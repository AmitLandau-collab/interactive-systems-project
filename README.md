### HopTales-System
# Overview:
HopTales is an innovative web application designed to revolutionize the way parents engage their children with stories. By combining artificial intelligence with user interactivity, HopTales offers a unique storytelling experience that allows for deep personalization and child-led narrative progression.

# System Description:
in the main menu, users can choose to: 
1. create a new interactive story, random or by their personalized settings. after viewing the story users can rate it and choose to save it. every user has its own library of saved stories.

2. view their saved stories library, which is ranked by the rating they gave to the story. users can unsave stories in the feedback page after reading the story again

3. change and login into another user, or create a new one.
   
# Getting Started:
## Setting up the Development Environment:
Pre-requisites: Git and Anaconda. 

To install and run the code on your local machine, follow these steps:
1. ### Clone the repository
   First, clone the repository to your local machine using Git. Open a terminal and run the following command:
    ```bash
    git clone https://github.com/AmitLandau-collab/interactive-systems-project
    ```
2. ### Create and activate the conda environment
   After cloning the repository, navigate into the project directory:
    ```bash
    cd interactive-systems-project 
    ```
    Then, use the following command to create a conda environment from the environment.yml file provided in the project:
    ```bash
    conda env create -f environment.yml
    ```
    Activate the environment with the following command:
   ```bash
    conda activate HopTales_env
    ```
## Running the System:
To run the project, follow these steps:

To use the Gemini API in this project, you need to authenticate with your API key. Your personal key can be found in: https://ai.google.dev/.

Copy your key and run the following command in the terminal:
```bash
set API_KEY="YOUR_API_KEY"
```

To use  openAI Dall-E API in this project, you need to authenticate with your API key. your personal key can be found in:
https://platform.openai.com/docs/api/

Copy your key and run the following command in the terminal:
```bash
set OPENAI_API_KEY="your_api_key_here"
```

Run the command:
```bash
streamlit run HopTales.py
```

# Usage:
A detailed instruction on how to use the system is provided in the demo video: https://drive.google.com/file/d/1uoEPCDLdlYw5W13qmoPLwm2ifmjgR1Sr/view?usp=sharing

# Gemini API Usage:
The Gemini API is utilized for processing user inputs, generating personalized interactive children stories, and prompts for the openAI Dall-E model .

# OpenAI Dall-E Usage:
The Gemini API is utilized for processing user inputs, generating personalized plant recommendations, and providing care instructions. Relevant prompts are supplied on each page to receive suitable and personalized answers from users.
