# PROJECT DESCRIPTION:

A puppeteer network that controls other models behavior by dynamically inhibiting or stimulating its weights, enabling model steering.

In our project we train and test models on an MNIST dataset. The ground model A is a CNN that classifies digits 1-10 like in a traditional MNIST. We then train the  "Puppeteer" model B (MLP) to adjust the activations of model A during infrerence according to what input we want it to inhibit in model A. To be exact, after training the model A, we train the "Puppeteer" in a supervised fashion by providing it a single number from 1-10 which the model should discriminate by acting on model A's activations during inference such that it classifies all digits correctly, but never outputs the specified digit (i.e: if we input 1 into the Puppeteer model, the output should always be 2-10, but not 1).







### Testing the Puppeteer:
def test_puppeteer_model(test_loader):
    conv_model.eval()
    puppeteer_net.eval()
    total_samples = 0
    inhibited_correct = 0  # Times the inhibited digit was not predicted
    total_correct = 0      # Total correct predictions
    inhibited_incorrect = 0  # Times the inhibited digit was predicted (undesirable)

    for images, labels in test_loader:
        images = images.unsqueeze(1)
        if torch.cuda.is_available():
            images = images.cuda()
            labels = labels.cuda()

        batch_size = images.size(0)
        total_samples += batch_size

        # Randomly select digits to inhibit (ensuring they are different from the true labels)
        inhibited_digits = torch.randint(0, 10, (batch_size,)).to(labels.device)
        inhibited_digits = (inhibited_digits + (inhibited_digits == labels).long()) % 10

        # Convert inhibited digits to one-hot vectors
        inhibited_digits_one_hot = torch.zeros(batch_size, 10).to(images.device)
        inhibited_digits_one_hot.scatter_(1, inhibited_digits.view(-1, 1), 1)

        # Forward pass through conv_block of the CNN
        x = conv_model.conv_block(images)

        # Flatten the activations
        x = x.view(batch_size, -1)

        # Get adjustments from Puppeteer MLP
        adjustments = puppeteer_net(inhibited_digits_one_hot)

        # Adjust the activations
        x = x + adjustments

        # Pass adjusted activations through linear_block of the CNN
        output = conv_model.linear_block(x)

        # Get predicted labels
        _, predicted = torch.max(output.data, 1)

        # Update counts
        inhibited_correct += (predicted != inhibited_digits).sum().item()
        inhibited_incorrect += (predicted == inhibited_digits).sum().item()
        total_correct += (predicted == labels).sum().item()

    # Calculate percentages
    inhibited_correct_rate = (inhibited_correct / total_samples) * 100
    inhibited_incorrect_rate = (inhibited_incorrect / total_samples) * 100
    accuracy = (total_correct / total_samples) * 100

    print(f'Inhibited Correct Rate (Inhibited Digit Not Predicted): {inhibited_correct_rate:.2f}%')
    print(f'Inhibited Incorrect Rate (Inhibited Digit Predicted): {inhibited_incorrect_rate:.2f}%')
    print(f'Overall Accuracy: {accuracy:.2f}%')
test_puppeteer_model(test_loader)

