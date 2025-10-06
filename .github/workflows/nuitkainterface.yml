name: Build and Commit Lichess Bot

on:
  workflow_dispatch:

jobs:
  build-and-commit:
    runs-on: ubuntu-latest

    steps:
      # 1️⃣ Checkout Repo
      - name: Checkout Repo
        uses: actions/checkout@v3
        with:
          fetch-depth: 0

      # 2️⃣ Set up Python
      - name: Set up Python
        uses: actions/setup-python@v4
        with:
          python-version: "3.13"

      # 3️⃣ Install Dependencies
      - name: Install Dependencies
        run: |
          python -m pip install --upgrade pip wheel
          pip install -r requirements.txt
          pip install nuitka

      # 4️⃣ Install Build Tools
      - name: Install Build Tools
        run: |
          sudo apt update
          sudo apt install -y build-essential python3-dev

      # 5️⃣ Compile user_interface.py
      - name: Compile Bot
        run: |
          python3 -m nuitka \
            --onefile \
            --output-filename=botli-linux \
            user_interface.py

      # 6️⃣ List Build Output
      - name: List Build Output
        run: ls -la

      # 7️⃣ Commit and Push to main
      - name: Commit and Push Bot
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        run: |
          git config --global user.name "github-actions"
          git config --global user.email "github-actions@github.com"
          git add botli-linux config.yml
          git commit -m "Update botli-linux and config.yml" || echo "No changes to commit"
          git push origin main
