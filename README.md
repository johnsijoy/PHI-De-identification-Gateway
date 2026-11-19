
## PHI De-identification Gateway

### About the Project
The PHI De-identification Gateway is a project developed to identify and hide personal information present in clinical notes before sharing the notes with an external Large Language Model (LLM).

Clinical notes may contain information such as patient names, dates, phone numbers, addresses, and medical record numbers. Sharing these details can create privacy risks.

In this project, I used a fine-tuned DistilBERT model along with regular expressions and rule-based techniques to detect and mask sensitive information. After processing the masked note using an LLM, the original information can be restored using a mapping stored locally.

### Project Objectives
- Identify personal information in clinical notes.
- Replace sensitive details with suitable placeholders.
- Keep the original information in a local mapping.
- Allow an LLM to process the masked clinical notes.
- Restore the original details in the LLM response.

### Technologies Used
- Python
- PyTorch
- DistilBERT
- Google Colab
- Regular Expressions
- Google Gemini API
- Microsoft Presidio
- Pandas
- NumPy

### How the Project Works
1. A clinical note is given as input.
2. The system identifies sensitive information in the note.
3. Detected information is replaced with placeholders such as `[PATIENT_NAME_001]` and `[PHONE_001]`.
4. The original values and placeholders are stored in a local mapping.
5. Only the masked note is sent to the LLM.
6. The LLM generates a response using the masked note.
7. The original information is restored in the response using the local mapping.

### Main Functions
1. deidentify(text) - Takes a clinical note as input and returns the masked note along with the local mapping.

2. rehydrate(response, mapping) - Takes the LLM response and local mapping as input and restores the original information.

### Model Information
- Base Model: DistilBERT
- Model Task: Token Classification
- Training Epochs: 3
- Learning Rate: 0.00002
- Total Parameters: 66,402,099 (approximately 66.4 million)
- Model Parameter Limit: Below 1 billion

### Evaluation
The project was tested using synthetic clinical notes.
The evaluation included:
- PHI detection using precision, recall, and F1-score.
- Checking for missed PHI information.
- Testing repeated names and identifiers.
- Testing dates and ages above 89.
- Checking de-identification and rehydration.
- Comparing results with Microsoft Presidio.
- Comparing clinical summaries generated from original and masked notes.

### Evaluation Results

       Method           Precision   Recall  F1-score 

 Hybrid Detector         1.0000     1.0000  1.0000   
 Validation Evaluation   0.9869     1.0000  0.9934  
 Microsoft Presidio      0.6752     0.4072  0.5080   

*Note: These are development and synthetic validation results, not independent benchmark results. Some test notes were used during rule refinement, and the additional validation annotations were generated from templates.*

### LLM Utility Testing
Five original synthetic clinical notes and their masked versions were used to generate summaries with Gemini.

The summaries were compared to check whether symptoms, clinical monitoring recommendations, and follow-up instructions were preserved. All 15 manually checked clinical information items were retained in the masked summaries.

### Project Limitations
- Some uncommon names or identifiers may not be detected.
- Rule-based detection may produce false positives.
- The system needs further testing with more varied synthetic notes.
- The current evaluation does not establish complete PHI detection or real-world privacy protection.
- This project is an educational prototype and is not certified as HIPAA-compliant.

### How to Run
1. Open the project notebook in Google Colab.
2. Install the required Python libraries.
3. Load the trained DistilBERT model.
4. Run the notebook cells in order.
5. Test the `deidentify()` and `rehydrate()` functions.
6. Configure the Gemini API key through Colab Secrets to run the optional LLM testing.

### References
- https://huggingface.co/docs/transformers/en/model_doc/distilbert
- https://www.hhs.gov/hipaa/for-professionals/special-topics/de-identification/index.html

