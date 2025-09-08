# 🟢 Using Huggingface

* We will be using huggingfaces spaces
* <mark style="color:purple;background-color:purple;">**Create a new space ⇒ Select CPU and done**</mark>
* <mark style="color:purple;background-color:purple;">**Create a new token of type write**</mark>
* <mark style="color:purple;background-color:purple;">**File from github needs to be pushed to huggingface space**</mark>
* <mark style="color:purple;background-color:purple;">**Search for hugggingfaces spaces action**</mark>
* <mark style="color:purple;background-color:purple;">**Copy this file and put it in the github as .github/workflow/main.yml**</mark>
* <mark style="color:purple;background-color:purple;">**Replace username, space name in yml**</mark>
* <mark style="color:purple;background-color:purple;">**in setting ⇒ Secrets and variable ⇒ Create respository secret key as HF\_TOKEN**</mark>
* Commit changes ⇒ CI/CD will run

**deploy.yml**

```yaml
name: Deploy to Hugging Face

on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repo
        uses: actions/checkout@v3
        with:
          fetch-depth: 0

      - name: Setup Python
        uses: actions/setup-python@v4
        with:
          python-version: "3.10"

      - name: Install dependencies
        run: |
          pip install -r requirements.txt

      - name: Push to Hugging Face
        env:
          HF_TOKEN: ${{ secrets.HF_TOKEN }}
        run: |
          # ✅ Safety check — fail if token is missing
          if [ -z "$HF_TOKEN" ]; then
            echo "❌ HF_TOKEN is not set. Please add it as a GitHub Actions secret."
            exit 1
          else
            echo "✅ HF_TOKEN is set. Proceeding with deploy"
          fi
          git config --global user.email "hiteshbwankhede@gmail.com"
          git config --global user.name "hiteshbwankhede"
          git remote set-url origin https://hiteshbwankhede:${HF_TOKEN}@huggingface.co/spaces/hiteshbwankhede/LLM-Prompt-Guard
          git push origin main --force
```

