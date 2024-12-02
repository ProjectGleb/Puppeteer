## Project Overview 🔎

Puppeteer is a mechanistic interpretebility network that controls other models behavior by dynamically inhibiting or stimulating its weights, enabling model steering.

<img width="707" alt="Puppeteer Net" src="https://github.com/user-attachments/assets/6af5ecd0-9e6f-460c-9ce3-e903a8aa99d2">

Weight inhibiting visualisation:
<img width="1231" alt="Weight inhibiting" src="https://github.com/user-attachments/assets/c7ec42b0-2bdf-4946-98c6-9bc75c84d15d">


In our project we train and test models on an MNIST dataset. The ground model A is a CNN that classifies digits 1-10 like in a traditional MNIST. We then train the  "Puppeteer" model B (MLP) to adjust the activations of model A during infrerence according to what input we want it to inhibit in model A. To be exact, after training the model A, we train the "Puppeteer" in a supervised fashion by providing it a single number from 1-10 which the model should discriminate by acting on model A's activations during inference such that it classifies all digits correctly, but never outputs the specified digit (i.e: if we input 1 into the Puppeteer model, the output should always be 2-10, but not 1).
