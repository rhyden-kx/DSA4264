# DSA4264 Reddit Analysis

## Members:
- Caleb Lee Heng Yi
- Denise Teh Kai Xin
- Lin Zhengjue Elisa
- Neleh Tok Ying Yun
- Sarah Goh Yue En

## Project Aim:
Create a pipeline for streamlined trend analysis for text-based data. Currently it is only reflected for Reddit comments-based data.

## Future Developments:
We hope to improve this pipeline to be generalized across all text data, simplifyiing the data analysis process for future teams and organisations.

## To Use:

1. Clone the repository:
    ```shell script
    git clone https://github.com/rhyden-kx/DSA4264.git
    ```

2. Navigate to this app's directory:
    ```shell script
    cd DSA4264
    ```
   
3. Create Venv:
    ```shell script
    python<version> -m venv <virtual-environment-name>
    ```
    Eg.
    ```shell script
    python3 -m venv env
    ```
    
4. Activate Virtual environment (env):
   For Mac OS
    ```shell script
    source env/bin/activate
    ```
    For Windows OS:
   ```shell script
    env\Scripts\activate
    ```
5. Install requirements:
    ```shell script
    pip install -r requirements.txt
    ```
6. Install GENSIM LDAMallet Package into DSA4264 directory:
    [ldaMallet.py link](https://github.com/piskvorky/gensim/blob/release-3.8.3/gensim/models/wrappers/ldamallet.py)

7. Run on your chosen Jupyter editor & select the [env] created as the kernel.
