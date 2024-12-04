## Project Overview 🔎

SymbioteNet is a mechanistic interpretebility network that controls other models behavior by dynamically inhibiting or stimulating its weights, enabling model steering. What is perhaps even more interesting is that through those interactions the Symbiote network learns the curcuit representations of the subseedary network, which allows you to observe which circuits are responsible for which inputs.
<img width="1000" alt="SymbiotNet" src="https://github.com/user-attachments/assets/265d3a0e-183d-44da-8dd6-7522ef1a0f0e">
Weight inhibiting visualisation:

<img width="1000" alt="Weight inhibiting" src="https://github.com/user-attachments/assets/c7ec42b0-2bdf-4946-98c6-9bc75c84d15d">


In our project we train and test models on an MNIST dataset. The ground model A is a CNN that classifies digits 1-10 like in a traditional MNIST. We then train the  "Symbiote" model B (MLP) to adjust the activations of model A during infrerence according to what input we want it to inhibit in model A. To be exact, after training the model A, we train the "Symbiote" in a supervised fashion by providing it a single number from 1-10 which the model should discriminate by acting on model A's activations during inference such that it classifies all digits correctly, but never outputs the specified digit (i.e: if we input 1 into the Symbiote model, the output should always be 2-10, but not 1).
