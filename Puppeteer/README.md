# PROJECT DESCRIPTION:

Model prompt:
I’m thinking of a research direction for model goal conditioning. Specifically I’m thinking of freezing the models parameters to direct it to explore desired concepts. 

The idea is this. You train two MNIST models.  Model A to predict numbers 1-10, the model B to predict numbers 1-8. You then train a model C to take in activations from the two models and try to predict the difference between them. I.e: model parameters in model A that are responsible for generating numbers 9 & 10. 

Given the model C learns the circuits in the model A that are respondible for numbers 9 & 10, you can now effectively freeze these activations, thereby conditioning to only generate numbers 1-8. 

Im trying to understand where this idea can be applicable. Can you please give me some suggestions.



------------------
Semi-related project:


This project aims to develop a method for understanding what training data neural networks (particularly CNNs) sample during inference by analyzing their internal representations. 
The core approach is to:

        * Extract activation patterns from different layers of a CNN during inference.
        * Map these activations to a common embedding space alongside the training data embeddings.
        * Use cosine similarity to identify which training examples most strongly correlate with the model's internal representations during inference.

This method would essentially reveal what training examples the model is 'remembering' or 'referencing' when making predictions on new inputs. 

This approach would help determine:
Which training examples most influenced a particular prediction?
What patterns in the training data is the model relying on?
How does the model's internal representation space relate to its training examples?

This can be useful in:
- Bias identification & Freezing
- Source backtracking 
- Model / Data debugging