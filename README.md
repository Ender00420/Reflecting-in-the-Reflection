# An Artificial Intelligence-Based System for Automatic Reflection Question Generation in Educational Settings

## Overview
This project contains the code and data for the thesis "An Artificial Intelligence-Based System for Automatic Reflection Question Generation in Educational Settings". The project focuses on using OpenAI's GPT-4o-mini model in Socratic dialogue to generate reflection questions for educational purposes. The system is designed to assist educators in creating engaging and thought-provoking questions that can enhance the learning experience.

Please note that the provided notebooks function as a proof of concept and are not intended for full production use. The code may require further optimization and testing before being deployed in a real-world educational setting. Also, be aware that repetition of the process of generation and evaluation of the experiment can take several hours based on the parameters and materials set. **For single question generation, use the provided demo notebook which takes only a few moments.**

## Project Structure
```plaintext
.
├── README.md
├── requirements.txt
├── Data/
│   ├── socratic_dialogue_loop.ipynb = Notebook used for generation of the dialogues
│   ├── free10iter/ 
│   │   ├── Prompts/ = Prompt used for the generation of the dialogues for the dynamic number of iterations
│   │   ├── free10iter.json = JSON file with the dialogues generated for the dynamic number of iterations
│   ├── fixed10iter/
│       ├── Prompts/ = Prompt used for the generation of the dialogues for the fixed number of iterations
│       ├── fixed10iter.json = JSON file with the dialogues generated for the fixed number of iterations
├── Demo/
│   ├── socratic_dialogue_demo.ipynb = Notebook simulating generation of a single question using Socratic dialogue
│   ├── Prompts/ = Directory containing the prompts used for the generation of the dialogues
├── Evals/
│   ├── BestCombination/ = Experiment to find the best combination of parameters for the generation of the dialogues
│   ├── BestIter/ = Experiment to find the best number of fixed iterations of the dialogue
│   ├── FreeVSFixed/ = Experiment to compare the dynamic and fixed number of iterations
│   ├── NoneVSFree/ = Experiment to determine the effect of dialogue on the generation of the questions
├── Materials/ = Directory containing the materials used for the generation of the dialogues
├── Thesis/ = Directory containing the LaTeX files used for the generation of the thesis
```

## Requirements
- **Python** 3.10 or higher
- A virtual environment tool (`venv` or `conda`)

### Setup Virtual Environment
```bash
# from repo root
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

### Configuration
Before running any notebooks or scripts, **you must supply your OpenAI API key** with enough credits. Set the key in the file `openai.txt` located in the root directory. The file should contain only the key, without any additional text or formatting.

## Demo
A demo notebook simulating generation of a single question using Socratic dialogue is provided in the `Demo` directory as `socratic_dialogue_demo.ipynb`. The notebook contains explanations of parameters and functions to print the generated dialogue. More detailed explanation of the parameters can be found in the "Generation of Reflection Questions" section below.

### Quickstart
1. Activate your virtual environment (see “Setup Virtual Environment”).  
2. Launch Jupyter Lab or Notebook:  
   ```bash
   jupyter lab
   # or
   jupyter notebook
   ```
3. Open `Demo/socratic_dialogue_demo.ipynb`.  
4. Edit the parameters (see "Generation of Reflection Questions" section below) 
5. Click **Run All** to start generation.

### Generation of Reflection Questions

Different models can be used in the generation and evaluation, though note that not all models have the same capabilities. Most importantly, some models (mainly older reasoning models) cannot use files as input, so combinations which use files will report an error. Another important consideration are rate limits (tokens per minute and calls per minute) which limit the number of calls to the API, potentially increasing the time needed for the generation and evaluation as large files add multiply the token count significantly. The LLM calling function should be able to handle this, but the time taken for generation and evaluation will increase. The model used in the generation was `gpt-4o-mini`, while the evaluation was done using `o4-mini`. Using these models no rate limit errors were encountered.

The system is simulated in jupyter notebook `socratic_dialogue_demo.ipynb`, located in the `Demo` directory. The notebook contains the main code for generating reflection questions with different combinations of parameters. The parameters for the generation can be set in the notebook, briefly described below:
- `model_used`: The name model used for the generation. Use `list_models` function to see the available models (not all models can be used for this task, see [OpenAI Models Reference](https://platform.openai.com/docs/models) for more information).
- `topic`: The overall topic of the target question.
- `concepts`: The concepts that should be incorporated in the question. If more than one is provided, the LLM is ordered to use one or few randomly.
- `student_level`: The knowledge level of the student. Should be in form of a grade (e.g. 5th grade)
- `sources`: Additional sources that should be used in the generation. These files need to be uploaded to the OpenAI API to be usable by the models (you can use the `file_upload` function). Place here in a list the IDs of the files (the first word printed by the `list_files` function). The sources have to be of a type supported by the API and LLM.
- `combination`: The combination of dialogue settings used in the generation. This controls what is used in the dialogue and its length. The combinations are 3 letters long, where:
    - d/s = dynamic dialogue length / static dialogue length
    - l/w = provide student level / without student level
    - m/w = provide additional sources / without additional sources
    - Example: `dlm` = dynamic dialogue length, with student level and with additional sources
- `max_iterations`: Maximum number of iterations for the generation. The set maximum of iterations a single dialogue can have until it is cut. This limit is also used with the dynamic iterations, so it should be set higher than the expected number of iterations. During the experiments the average amount of iterations with dynamic iterations was about 2.3 with highest being 4, so setting it to 5 could be sufficient. (During experiments this was set to 10).

#### Parameters for Batch Generation
These parameters were used in the batch generation of the dialogues and questions, which is done in the `socratic_dialogue_loop.ipynb` notebook located in the `Data` directory. The parameters are similar to the ones used in the demo notebook, with some additional parameters:
- `directory`: Directory which contains the prompts used for the generation. `Fixed10iter` contains prompts for fixed amount of iterations (not necessarily 10). `Free10iter` contains prompts for dynamic number of iterations (prompts of teacher-educator have an additional instruction). The function reading the prompts and combination has a specific format for the prompts, so make sure to follow the format used in the `Prompts` directory if you want to use own prompts or combinations. If you want to exclude a specific combination, simply remove the folder containing the combination from the `Prompts` directory. Each folder in the `Prompts` directory contains file with prompts `prompts.txt` and `combination.json`. The `prompts.txt` file contains the prompts edited for this specific combination (should always contain the three prompts) while the `combination.json` file contains information about the combination where combination is:
    - dlm = dialogue with student level and materials
    - dlw = dialogue with student level and without materials
    - dwm = dialogue without student level and with materials
    - dww = dialogue without student level and without materials
- `results`: Name of the file where the results will be saved. (Specify the path and file type as `json`)
- `attempts`: Number of attempts (questions) to generate for each combination

## Evaluation of Reflection Questions

The `Evals` folder contains the notebooks and results of the evaluation of the generated questions. Separate notebooks were made for each experiment as each experiments has unique criteria and evaluation methods. Each directory contains a copy of the source data used in the evaluation as well as csv files with its results.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

## Acknowledgements
This software was created as part of a master's thesis at the [Faculty of Information Technology, Czech Technical University in Prague](https://fit.cvut.cz/en) and with cooperation from [AI dětem](https://www.aidetem.cz/).

<img src="logo-ctu-fit-blue.jpg" width="150" height="49" />
