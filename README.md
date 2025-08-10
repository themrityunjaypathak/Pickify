<h2 align='center'>Pickify : Movie Recommender System</h2>

<img src='https://github.com/user-attachments/assets/402f6ff0-b0f0-4382-827b-b7b06e33961d' title='Banner'>

## Table of Contents
- [Problem Statement](#problem-statement)
- [Overview](#overview)
- [Working](#working)
- [Features](#features)
- [Setup](#setup)
- [Folder Structure](#folder-structure)
- [Challenges & Solutions](#challenges--solutions)
- [Impact](#impact)
- [Future Improvements](#future-improvements)
- [License](#license)

<hr>

## Problem Statement
- With the rise of streaming services, viewers now have access to thousands of movies across platforms.
- As a result, many viewers spend more time browsing than actually watching.
- This problem can lead to frustration, lower satisfaction and less time spent on the platform.
- Which can impact both the user experience and business performance.

<hr>

## Overview
- A production-ready content-based movie recommender system built with clean coding practices, modular design, proper version control and deployed as a web application.
- It analyzes metadata of 5000+ movies such as genres, cast, crew, keywords and overview to recommend top 5 similar movies based on a user-selected movie.
- The system uses techniques like [`CountVectorizer`](https://scikit-learn.org/stable/modules/generated/sklearn.feature_extraction.text.CountVectorizer.html) for text vectorization and [`cosine_similarity`](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.pairwise.cosine_similarity.html) to find similarity between movies.
- The project not only focuses on functionality but on building a clean, scalable and production ready solution, applying industry standard practices.

<hr>

## Working

- The dataset contains metadata for each movie including keywords, genres, cast, crew and overview.

<img src='https://github.com/user-attachments/assets/7196153c-f0a8-46cb-b47f-204b5db0cf46' title='Screenshot-1'>

- All these features are combined into a new column called `tags` to create a unified representation of each movie.

<img src='https://github.com/user-attachments/assets/9f019c4c-0c1b-4f87-acdc-1253dd791eea' title='Screenshot-2'>

- Text preprocessing is applied to the `tags` column :
  - All text is converted to lowercase (e.g., `"Action, Thriller"` becomes `"action, thriller"`).
  - Spaces between words are removed (e.g., `"action movie"` becomes `"actionmovie"`).
  - Stemming is performed using [`PorterStemmer`](https://www.nltk.org/howto/stem.html) to reduce words to their root form.
  
<img src='https://github.com/user-attachments/assets/54710f7c-b354-480f-b4be-b21b6333bacb' title='Screenshot-3'>

- CountVectorizer is used to convert the `tags` column into numerical feature vectors.
- Cosine similarity is used to calculate similarity between the vector representations of all the movies.
- The resulting similarity matrix is serialized and saved as a `.pkl` file for efficient loading during recommendation.
- A Streamlit web application is built to provide an interactive interface for movie selection and recommendation :
  - User select a movie from the dropdown list.
  - The system recommends top 5 most similar movies based on the similarity score.
- Movie posters are fetched using the TMDB API to enhance the visual appeal of the recommendations.
 
<a href='https://pickify.streamlit.app/' title='Pickify'><img src='https://github.com/user-attachments/assets/b2c8e685-565d-4a09-a49a-4323cefd8852'/></a>

<hr>

## Features

### 1. Modular Design with Reusable Codes
- The project follows a modular approach by organizing modules into a dedicated `utils/` directory.
- Each module in `utils/` directory is responsible for a specific task and includes :
  - Clear docstrings explaining functionality, expected inputs/outputs, returns and raises.
  - Robust exception handling for better error tracing and debugging.
- Following the DRY (Don't Repeat Yourself) principle, this design : 
  - Reuse functions across notebooks and scripts without rewriting code.
  - Save development time and reduce redundancy.
- The `utils/` directory also includes `__init__.py` file in it, which serves a few important purposes in Python :
  - The `__init__.py` file tells Python to treat the directory as a package, not just a regular folder.
  - Without it, Python won't recognize the folder as a package.

### Add `utils` to PYTHONPATH
- To access these utility modules anywhere in the project, add the following snippet at the top of your script :
```python
import sys, os
sys.path.append(os.path.abspath("../utils"))
```

### Example
- This is one of the functions I added in my project as `export_data.py` module in `utils` directory.
```python
import os
import pandas as pd

def export_as_csv(dataframe, folder_name, file_name):
    """
    Exports a pandas DataFrame as a CSV file to a specified folder.

    Parameters:
        dataframe (pd.DataFrame): The DataFrame to export.
        folder_name (str): Name of the folder where CSV file will be saved.
        file_name (str): Name of the CSV file. Must end with '.csv' extension.

    Returns:
        None

    Raises:
        TypeError: If input is not a pandas DataFrame.
        ValueError: If file_name does not end with '.csv' extension.
        FileNotFoundError: If folder does not exist.
    """
    try:
        if not isinstance(dataframe, pd.DataFrame):
            raise TypeError("Input must be a pandas DataFrame.")
        if not file_name.lower().endswith('.csv'):
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

<hr>

### 2. Dynamic Path Handling with `os.path`
- Instead of hardcoding file paths, the project uses Python's built-in `os` module to handle file paths dynamically.
- This improves code flexibility, ensuring the code runs smoothly across different systems and environments.

### Key Benefits
- Automatically adapt to the system directory structure.
- Prevent `FileNotFoundError` caused by rigid, hardcoded paths.
- Make deployment and collaboration easier without manual path updates.

### Example
```python
current_dir = os.getcwd()
parent_dir = os.path.dirname(current_dir)
folder_path = os.path.join(parent_dir, folder_name)
file_path = os.path.join(folder_path, file_name)
```

<hr>

### 3. Clean Commit History with `nbstripout`
- Integrated [`nbstripout`](https://github.com/kynan/nbstripout) with Git to automatically remove Jupyter notebooks output before committing.
- It helps maintain a clean and readable commit history by :
  - Avoiding large, unreadable diffs caused by cell outputs.
  - Keeping only code and markdown content under version control.
- Especially useful when pushing to remote repositories, as it reduces clutter and improves code readability.

### Setup `nbstripout` for Jupyter Notebooks
#### 1. Install `nbstripout`
- Install it in your virtual environment using `pip`.
```bash
pip install nbstripout
```

#### 2. Enable it for your Git Repository
- This sets up a Git filter to strip notebook output automatically on commits.
```bash
nbstripout --install
```
   
#### 3. Verify it's Working  
- Commit a notebook and observe that the outputs are removed from the committed version.

<hr>

### 4. Secure API Key with `st.secrets`
- The project uses Streamlit's `st.secrets` feature to handle the TMDB API key securely during local development.
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
> You can add it to .gitignore to ensure it's excluded from version control.
> 
> When deploying to streamlit, the API key must be added via the GUI, not through the `secrets.toml` file.

<img src='https://github.com/user-attachments/assets/529ae232-8635-479d-9b8f-d089b5688e7a' title='Streamlit UI'>

<hr>

### 5. Accessing Large Files with `gdown`
- In the project, a similarity matrix is computed to recommend movies.
- But due to its high dimensionality, size of the matrix becomes very large and exceeds GitHub's size limitations.
- GitHub restricts uploads larger than 100MB in public repositories, making it unsuitable for storing large files.
- While Git LFS (Large File Storage) is one option, it can be complex to configure and manage.

### Solution with `gdown`
- To address this issue, the matrix file is :
  - Uploaded to Google Drive.
  - Downloaded at runtime using [`gdown`](https://pypi.org/project/gdown/) library.
  - Stored locally on streamlit server when the app runs.
- This approach ensures :
  - Compatibility with GitHub without needing Git LFS.
  - A hassle-free experience for cloning the repository or running the app across environments.

### Example
```python
import os
import gdown
import pickle

# Step 1: Define the Google Drive file ID
file_id = 'your_file_id_here'

# Step 2: Set the desired file name for the downloaded file
output = 'similarity.pkl'

# Step 3: Construct the direct download URL from the file ID
url = f'https://drive.google.com/uc?id={file_id}'

# Step 4: Check if the file already exists locally
# If not, download it from Google Drive using gdown
if not os.path.exists(output):
    gdown.download(url, output, quiet=False)

# Step 5: Open the downloaded file in read binary mode
# and load the similarity matrix using pickle
with open('similarity.pkl', 'rb') as f:
    similarity = pickle.load(f)
```

<hr>

## Setup

Follow these steps carefully to setup and run the project on your local machine :

### 1. Clone the Repository
First, you need to download the project from GitHub to your local system.
```bash
git clone https://github.com/TheMrityunjayPathak/Pickify.git
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
.venv\Scripts\activate
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
> The `.streamlit/` folder contains streamlit configuration settings.
>
> But it's not necessary to included in your project until required.

#### config.toml
- The `config.toml` file contains configuration settings such as the server settings, theme preferences, etc.
```bash
[theme]
base="dark"
primaryColor="#FD3A84"
backgroundColor="#020200"
```

#### secrets.toml
- The `secrets.toml` file contains sensitive information like API keys, database credentials, etc.
```bash
[TMDB]
api_key = "your_tmdb_api_key_here"
```

> [!IMPORTANT]
> Make sure not to commit your `secrets.toml` to GitHub or any public repositories.
> 
> You can add it to `.gitignore` to ensure it's excluded from version control.

### 6. Run the Streamlit Application
After everything is setup, you can run the streamlit application :
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
movie_recommender_system/
|
├── .streamlit/             # Streamlit Configuration Files
├── raw_data/               # Original Datasets
├── clean_data/             # Preprocessed and Cleaned Datasets
├── notebooks/              # Jupyter Notebooks for Preprocessing and Vectorization
├── images/                 # Images used in Streamlit Application
├── utils/                  # Modular Python Scripts
├── app.py                  # Main Streamlit Application
├── requirements.txt        # List of required libraries for the Project
├── README.md               # Detailed documentation of Project
├── LICENSE                 # License specifying permissions and usage rights
├── .gitignore              # All files and folders excluded from Git Tracking
```

<hr>

## Challenges & Solutions

| Challenge | Solution |
|:---|:---|
| Keeping Commits Clean | Used `nbstripout` to remove notebooks output before committing. | 
| Managing Large Files | Used Google Drive with `gdown` to load large files effectively. |
| Hiding Sensitive API Keys | Used `st.secrets` to securely store and access sensitive information. |
| Reusability and Scalability | Structured the project with modular code in `utils/` package. |
| Dynamic File Paths | Used the `os` module for dynamic and platform-independent path handling. |

<hr>

## Impact
If this system gets scaled and integrated with a streaming service, this could :
- Reduce the time users spend choosing what to watch.
- Increase user engagement, watch time and customer satisfaction.
- Help streaming platforms retain users by offering better personalized content.

<hr>

## Future Improvements

#### 1. Enhanced Tag Generation
- Currently, tags are generated equally from cast, crew, keywords, genres and overview.
- We can improve this by applying feature importance or weighting certain features.
- This can be done by multiplying certain column values to give them higher importance.

#### 2. User Preferences Integration
- Add user-based data to provide more personalized recommendations.
- Collaborative filtering can suggest movies based on similar user behaviour.
- This will make the recommender system more user-centric.

#### 3. Real-Time Data Updates
- Fetch movies data from external sources to ensure the movies database is always up-to-date.
- This would allow to recommend the latest releases and remove outdated movies automatically.

#### 4. Improved Similarity Metrics
- Instead of just Cosine Similarity, we can experiment with other advanced similarity measures,
- Like Jaccard Similarity, TF-IDF or Word2Vec for capturing semantic meaning in the movie descriptions.

#### 5. Interactive User Interface
- Enhance the user experience by providing filters to choose movies based on genres, actors or directors.

<hr>

## License

This project is licensed under the [MIT License](LICENSE). You are free to use and modify the code as needed.

<div align='left'>
  
**[`^        Scroll to Top       ^`](#pickify--movie-recommender-system)**

</div>
