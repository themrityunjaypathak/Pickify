<h2 align='center'>Pickify : Movie Recommender System</h2>

<p align="center">
  <a href="https://pickify.streamlit.app/" target="_blank">
    <img src="https://img.shields.io/badge/Live%20Demo-Streamlit-FF4B4B?style=flat&logo=streamlit&logoColor=white">
  </a>

  <a href="https://www.python.org/" target="_blank">
    <img src="https://img.shields.io/badge/Python-v3.11-3776AB?style=flat&logo=python&logoColor=white">
  </a>

  <a href="https://pandas.pydata.org/" target="_blank">
    <img src="https://img.shields.io/badge/Pandas-v2.2-150458?style=flat&logo=pandas&logoColor=white">
  </a>

  <a href="https://streamlit.io/" target="_blank">
    <img src="https://img.shields.io/badge/Streamlit-v1.44-FF4B4B?style=flat&logo=streamlit&logoColor=white">
  </a>

  <a href="https://scikit-learn.org/stable/" target="_blank">
    <img src="https://img.shields.io/badge/scikit--learn-v1.5-F7931E?style=flat&logo=scikit-learn&logoColor=white">
  </a>

  <a href="https://git-scm.com/" target="_blank">
    <img src="https://img.shields.io/badge/Git-v2.47-F05032?style=flat&logo=git&logoColor=white">
  </a>

  <a href="https://developer.themoviedb.org/docs" target="_blank">
    <img src="https://img.shields.io/badge/TMDB-API-01B4E4?style=flat&logo=themoviedatabase&logoColor=white">
  </a>
</p>

<img src="https://github.com/user-attachments/assets/402f6ff0-b0f0-4382-827b-b7b06e33961d" title="Banner">

## Table of Contents
- [Problem Statement](#problem-statement)
- [Overview](#overview)
- [How it Works](#how-it-works)
- [Workflow](#workflow)
- [Impact](#impact)
- [Features](#features)
- [Setup](#setup)
- [Folder Structure](#folder-structure)
- [Challenges & Solutions](#challenges--solutions)
- [Future Improvements](#future-improvements)
- [License](#license)

<hr>

## Problem Statement
- With the rise of streaming services, users now have access to thousands of movies across platforms.
- As a result, many viewers spend more time browsing than watching content.
- This leads to frustration, lower satisfaction, and reduced watch time on the platform.
- Over time, this impacts both user retention and platform engagement.

<hr>

## Overview
- Built a content-based movie recommender system trained on 5,000+ movie metadata records.
- Generated the top 5 similar titles for any selected movie in under 3 seconds using cosine similarity.
- Integrated the TMDB API to dynamically fetch and display movie posters, improving the user experience.
- Deployed the system as a Streamlit web app, enabling users to explore personalized movie suggestions.

<hr>

## How it Works

- The dataset contains metadata for each movie including title, keywords, genres, cast, crew, and overview.

<details>
<summary>Click Here to view the Data</summary>
<br>

<img src='https://github.com/user-attachments/assets/c945f7a6-2f3b-4f66-bbeb-89ca1a071073' title='Screenshot-1'>
</details>

- All the features are combined into a new column called `tags` to create a unified representation for each movie.

<details>
<summary>Click Here to view the Data</summary>
<br>

<img src='https://github.com/user-attachments/assets/18fae5c0-1885-455f-a2ea-019aaa1741aa' title='Screenshot-2'>
</details>

- Text preprocessing is applied to the `tags` column :
  - All text is converted to lowercase (e.g., `"Action, Thriller"` becomes `"action, thriller"`).
  - Spaces between words are removed (e.g., `"action movie"` becomes `"actionmovie"`).
  - Stemming is performed to reduce words to their root form (e.g., `"running"` becomes `"run"`).

<details>
<summary>Click Here to view the Data</summary>
<br>

<img src='https://github.com/user-attachments/assets/b19384f9-62e4-40dd-806b-d23038cf2c5c' title='Screenshot-3'>
</details>

- `CountVectorizer` is used to convert the `tags` column into numerical feature vectors.
- `cosine_similarity` is used to calculate similarity between the vector representations of all the movies.
- The resulting similarity matrix is serialized and saved as a `.pkl` file for efficient loading during recommendation.
- A Streamlit web application is built to provide an interactive interface for movie selection and recommendation :
  - User selects a movie from the dropdown list.
  - The system recommends the top 5 most similar movies based on the similarity scores.
- Movie posters are fetched using the TMDB API to enhance the visual appeal of the recommendations.

Access the Streamlit Web Application [here](https://pickify.streamlit.app/) or Click on the Image below.

<a href='https://pickify.streamlit.app/' title='Pickify'><img src='https://github.com/user-attachments/assets/f25be709-bae6-4aaf-8f80-569d26f835a4'/></a>

<hr>

## Workflow

<img alt="Mermaid Diagram" src="https://github.com/user-attachments/assets/aa5a4e5b-2a44-4aa3-a88a-84a14b220ba7">

<hr>

## Impact
- Reduced browsing time by instantly suggesting the top 5 most similar movies for any selected title.
- Delivered movie recommendations in under 3 seconds, ensuring a fast and smooth user experience.
- Improved content engagement by guiding users toward relevant titles instead of manual browsing.
- Served 100+ users through a deployed web app, turning a notebook model into a live recommendation system.

<hr>

## Features

### 1. Modular Design with Reusable Code
- The project follows a modular approach by organizing modules into a dedicated `utils/` directory.
- Each module in the `utils/` directory is responsible for a specific task and includes :
  - Clear docstrings explaining functionality, expected inputs/outputs, returns, and raises.
  - Robust exception handling for better debugging and maintainability.
- Following the DRY (Don't Repeat Yourself) principle, this design :
  - Reuses functions across notebooks and scripts without rewriting code.
  - Saves development time and reduces redundancy.
- The `utils/` directory also includes an `__init__.py` file, which serves a few important purposes in Python :
  - The `__init__.py` file tells Python to treat the directory as a package, not just a regular folder.
  - Without it, Python won't recognize the folder as a package.

### Add `utils` to PYTHONPATH
- To access these utility modules anywhere in the project, add the following snippet at the top of your script :
```python
import sys, os
sys.path.append(os.path.abspath("../utils"))
```

### Example
- This is one of the functions I added to my project as the `export_data.py` module in the `utils/` directory.
<details>
<summary>Click Here to View Example Function</summary>
<br>

```python
import os
import pandas as pd

def export_as_csv(dataframe, folder_name, file_name):
    """
    Exports a pandas DataFrame as a CSV file to a specified folder.

    Parameters:
        dataframe (pd.DataFrame): The DataFrame to export.
        folder_name (str): Name of the folder where CSV file will be saved.
        file_name (str): Name of the CSV file. Must end with ".csv" extension.

    Returns:
        None

    Raises:
        TypeError: If input is not a pandas DataFrame.
        ValueError: If file_name does not end with ".csv" extension.
        FileNotFoundError: If folder does not exist.
    """
    try:
        if not isinstance(dataframe, pd.DataFrame):
            raise TypeError("Input must be a pandas DataFrame.")
        if not file_name.lower().endswith(".csv"):
            raise ValueError("File name must end with '.csv' extension.")
        
        current_dir = os.getcwd()
        parent_dir = os.path.dirname(current_dir)
        folder_path = os.path.join(parent_dir, folder_name)
        file_path = os.path.join(folder_path, file_name)

        if not os.path.isdir(folder_path):
            raise FileNotFoundError(f"Folder '{folder_name}' does not exist.")

        dataframe.to_csv(file_path, index=False)
        print(f"Successfully exported the DataFrame as '{file_name}'")
    except TypeError as e:
        print(e)
    except ValueError as e:
        print(e)
    except FileNotFoundError as e:
        print(e)
```
</details>

<hr>

### 2. Dynamic Path Handling with `os.path`
- Instead of hardcoding file paths, the project uses Python's built-in `os` module to handle paths dynamically.
- This improves code flexibility, ensuring it runs smoothly across different systems and environments.

### Key Benefits
- Automatically adapts to the system's directory structure.
- Prevents `FileNotFoundError` caused by rigid, hardcoded paths.
- Makes deployment and collaboration easier without manual path updates.

<details>
<summary>Click Here to View Code Snippet</summary>
<br>
  
```python
current_dir = os.getcwd()
parent_dir = os.path.dirname(current_dir)
folder_path = os.path.join(parent_dir, folder_name)
file_path = os.path.join(folder_path, file_name)
```
</details>

<hr>

### 3. Secure API Key with `st.secrets`
- The project uses Streamlit's `st.secrets` feature to securely manage the TMDB API key during development.
- A `secrets.toml` file is placed inside the `.streamlit/` directory, storing the API key in the following format :
```toml
[tmdb]
api_key = "your_api_key_here"
```
- The API key is accessed in code using :
```python
api_key = st.secrets["tmdb"]["api_key"]
```

> [!CAUTION]
> The `secrets.toml` file should not be pushed to a public repository to avoid exposing sensitive credentials.
>
> You can add it to `.gitignore` to ensure it's excluded from version control.
> 
> When deploying to Streamlit, the API key must be added via the GUI, not through the `secrets.toml` file.

<hr>

### 4. Accessing Large Files with `gdown`
- In the project, a similarity matrix is computed to recommend movies.
- However, due to its high dimensionality, the matrix becomes very large and exceeds GitHub's size limitations.
- GitHub restricts uploads larger than 100MB in public repositories, making it unsuitable for storing large files.
- While Git LFS (Large File Storage) is one option, it can be complex to configure and manage.

### Solution with `gdown`
- To address this issue, the matrix file is :
  - Uploaded to Google Drive.
  - Downloaded at runtime using the [`gdown`](https://pypi.org/project/gdown/) library.
  - Stored locally on the Streamlit server when the app runs.
- This approach ensures :
  - Compatibility with GitHub without needing Git LFS.
  - Hassle-free experience for cloning the repository or running the app across different environments.

<details>
<summary>Click Here to View Code Snippet</summary>
<br>

```python
import os
import gdown
import pickle

# Step 1 : Define the Google Drive file ID
file_id = "your_file_id_here"

# Step 2 : Set the desired file name for the downloaded file
output = "similarity.pkl"

# Step 3 : Construct the direct download URL from the file ID
url = f"https://drive.google.com/uc?id={file_id}"

# Step 4 : Check if the file already exists locally
# If not, download it from Google Drive using gdown
if not os.path.exists(output):
    gdown.download(url, output, quiet=False)

# Step 5 : Open the downloaded file in read binary mode
# and load the similarity matrix using pickle
with open("similarity.pkl", "rb") as f:
    similarity = pickle.load(f)
```
</details>

<hr>

## Setup

Follow these steps carefully to set up and run the project on your local machine :

### 1. Clone the Repository
First, you need to clone the project from GitHub to your local system.
```bash
git clone https://github.com/themrityunjaypathak/Pickify.git
```

### 2. Setup a Virtual Environment
To avoid version conflicts and keep your project isolated, create a virtual environment.

On Windows :
```bash
python -m venv .venv
```

On macOS/Linux :
```bash
python3 -m venv .venv
```

### 3. Activate the Virtual Environment
After setting up the virtual environment, activate it to begin installing dependencies.

On Windows :
```bash
.\.venv\Scripts\activate
```

On macOS/Linux :
```bash
source .venv/bin/activate
```

### 4. Install the Project Dependencies
Now, install all the required libraries inside your virtual environment using the `requirements.txt` file.
```bash
pip install -r requirements.txt
```

> [!TIP]
> It's a good idea to upgrade `pip` before installing dependencies to avoid compatibility issues.
```bash
pip install --upgrade pip
```

### 5. Setup Streamlit Configuration (Optional) 
> [!NOTE]
> The `.streamlit/` folder contains Streamlit configuration settings.
>
> However, it is not necessary to include it in your project unless required.

#### config.toml
- The `config.toml` file contains configuration settings such as the server settings, theme preferences, etc.
```toml
[theme]
base="dark"
primaryColor="#FD3A84"
backgroundColor="#020200"
```

#### secrets.toml
- The `secrets.toml` file contains sensitive information like API keys, database credentials, etc.
```toml
[tmdb]
api_key = "your_tmdb_api_key_here"
```

> [!IMPORTANT]
> Make sure not to commit your `secrets.toml` to GitHub or any public repositories.
> 
> You can add it to `.gitignore` to ensure it's excluded from version control.

### 6. Run the Streamlit Application
After everything is setup, you can run the Streamlit application :
```bash
streamlit run app.py
```

### 7. Deactivate Virtual Environment
Once you're done working, you can deactivate your virtual environment :
```bash
deactivate
```

<hr>

## Folder Structure

```
Pickify/
|
├── .streamlit/             # Streamlit Configuration Files
├── raw_data/               # Original Dataset
├── clean_data/             # Preprocessed and Cleaned Dataset
├── notebooks/              # Jupyter Notebooks for Preprocessing and Vectorization
├── images/                 # Images used in the Streamlit Application
├── utils/                  # Modular Python Scripts
├── app.py                  # Main Streamlit Application
├── requirements.txt        # List of required libraries for the Project
├── README.md               # Detailed documentation of the Project
├── LICENSE                 # License specifying permissions and usage rights
├── .gitignore              # All files and folders excluded from Git Tracking
```

<hr>

## Challenges & Solutions

#### Challenge 1 : Dynamic File Paths
- **Solution :** Used Python's `os` module for dynamic, platform-independent path handling.

#### Challenge 2 : Reusability and Scalability
- **Solution :** Structured the project with modular scripts inside the `utils/` package.

#### Challenge 3 : Hiding Sensitive API Keys
- **Solution :** Used Streamlit `st.secrets` to securely store and access TMDB API credentials.

#### Challenge 4 : Managing Large Files
- **Solution :** Used Google Drive to host the serialized similarity matrix and download it at runtime using `gdown`.

<hr>

## Future Improvements

#### 1. Enhanced Tag Generation
- Currently, tags are generated with equal importance from cast, crew, keywords, genres, and overview.
- This can be improved by applying feature weighting to give more importance to influential attributes.
- For example, certain columns can be scaled or repeated to increase their impact on similarity calculations.

#### 2. User Preferences Integration
- Introduce user-based data to generate more personalized recommendations.
- Collaborative filtering can suggest movies based on similarities between user preferences and behavior.
- This would make the recommender system more adaptive and user-centric.

#### 3. Real-Time Data Updates
- Fetch movie data from external sources to keep the database continuously updated.
- This would enable recommending newly released movies and automatically removing outdated content.

#### 4. Improved Similarity Metrics
- Instead of relying only on cosine similarity, experiment with additional similarity techniques.
- Methods such as Jaccard similarity, TF-IDF, or Word2Vec could better capture semantic relationships.

#### 5. Interactive User Interface
- Improve the user experience by adding filters to explore movies based on genres, actors, or directors.

<hr>

## License

This project is licensed under the [MIT License](LICENSE). You are free to use and modify the code as needed.

<div align='left'>
  
**[`^        Scroll to Top       ^`](#pickify--movie-recommender-system)**

</div>
