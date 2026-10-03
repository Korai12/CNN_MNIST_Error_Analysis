# CNN MNIST Error Analysis

This project investigates how a convolutional neural network classifies handwritten digits from the MNIST dataset, with a focus on understanding where the model makes mistakes and whether one controlled change affects those mistakes.

## Problem Statement

The goal of the project is to train a CNN to recognise handwritten digits from 0 to 9 and then analyse its errors using a confusion matrix as the central question is:

Where does the CNN make its mistakes, and does one change to the model move them?

This is important because handwritten digit recognition can be used in areas such as postcodes and account numbers, where an incorrect prediction can have real consequences.

## Dataset

The project uses the MNIST handwritten digit dataset downloaded through OpenML where the dataset contains 70,000 grayscale images of handwritten digits and each image is 28 by 28 pixels and belongs to one of 10 classes representing the digits from 0 to 9.
The data was reshaped into 28 by 28 by 1 images, converted to float32 and normalised from the range 0 to 255 into the range 0 to 1.
A stratified train and test split was used, producing 56,000 training images and 14,000 test images. During training, 10 percent of the training data was used for validation.

## Method

The baseline CNN contains one convolutional layer with 32 filters, a max pooling layer, dropout of 0.3, a flatten layer, a dense layer with 64 neurons and a final output layer with 10 neurons. The model was trained using the Adam optimizer, sparse categorical crossentropy loss and accuracy as the evaluation metric. Early stopping was used to monitor validation loss and restore the best model weights. After evaluating the baseline model, a confusion matrix was used to identify the most common mistakes.

For the required controlled experiment, only one change was made which was the dropout rate was increased from 0.3 to 0.5. All other settings were kept the same so that the effect of stronger regularisation could be compared fairly.

## Results

The baseline model achieved approximately 98.58 percent test accuracy and its most common mistake was predicting 9 instead of 4, which happened 22 times. The next highest error count was tied, with 12 cases of 2 being predicted as 8 and 12 cases of 9 being predicted as 7.

The second model, using dropout of 0.5, achieved approximately 98.81 percent test accuracy, the 4 to 9 confusion decreased from 22 cases to 6, the 2 to 8 confusion decreased from 12 to 9 and the 9 to 7 confusion also decreased from 12 to 9.

Model 1 made approximately 199 test errors, while Model 2 made approximately 166.

## Interpretation

The controlled change produced only a small increase in overall test accuracy, from approximately 98.58 percent to 98.81 percent. However, the change had a much larger effect on the main error pattern as the 4 to 9 confusion dropped from 22 cases to 6, showing that the stronger dropout meaningfully changed where the model made mistakes.
This shows why it is useful to look at confusion matrices instead of only overall accuracy as a small difference in accuracy can still hide an important improvement in a specific type of error.

There are also ethical considerations as handwriting styles can vary between different people and groups, so a model trained on one dataset may not perform equally well on all real world handwriting. A confident but incorrect prediction could also cause problems in applications such as postcode or account number recognition as a result for important uses, the system should therefore include wider testing and human review or other safeguards.

## Reflection

The project worked well because the CNN models achieved high test accuracy and the confusion matrices made it easy to see which digit pairs caused the most problems. The controlled dropout experiment was especially useful because it showed that even a small improvement in overall accuracy can produce a much larger improvement in a specific type of error.

One of the more difficult parts was deciding whether the small differences in overall accuracy were meaningful. Looking only at the final accuracy was not enough, so the confusion matrices were important for understanding how the models were actually changing.

If I continued the project, I would repeat the experiments with multiple random seeds, use a dedicated validation set for model selection, inspect misclassified images individually, test the models on handwriting outside MNIST and experiment with data augmentation.



## Extra Experiments

After completing the required controlled experiment, I carried out two additional exploratory experiments, which where:

1. Model 3 used a deeper CNN by adding a second convolutional layer with 64 filters and a second max pooling layer, while keeping dropout at 0.3. As a reesult it achieved approximately 98.94 percent test accuracy and made about 148 errors.

2. Model 4 combined the deeper CNN architecture with dropout of 0.5 and it achieved the highest observed test accuracy at approximately 99.05 percent and made about 133 errors, which was the lowest approximate error count of all four models.

For the main confusion pairs, Model 4 reduced 4 to 9 errors to 6 cases and 2 to 8 errors to 3 cases. Model 3 performed best for 9 to 7, with only 3 cases compared with 7 in Model 4.

Because Models 3 and 4 were explored after the earlier test results had already been examined, their results should be treated as exploratory. In a stricter workflow, model selection would be based on validation data and the test set would ideally be evaluated only once after selecting the final model, also these two models were additional exploratory experiments and do not replace the official Model 1 versus Model 2 controlled comparison.
