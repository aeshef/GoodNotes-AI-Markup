# GoodNotes-AI (study_kb) ✨

Стэндэлон-папка для разметки макета страниц и обучения YOLO.

## Содержание

- [Структура папки 🗂️](#структура-папки-️)
- [Требования 📦](#требования-)
- [Быстрый старт для коллеги 🚀](#быстрый-старт-для-коллеги-)
- [Настройка в VS Code / Cursor 🧰](#настройка-в-vs-code--cursor-)
- [Процесс разметки ✍️](#annotator-workflow-details)
- [Обучение (owner) 🧪](#owner-workflow-training)
- [Заметки 🧠](#notes)

### Структура папки 🗂️

- data/ — place source PDFs here (subfolders allowed)
- dataset/
  - images/ — rendered pages for annotated samples
  - viz/ — previews with bounding boxes for QA
  - annotations.json — labels with metadata
  - yolo_iter/ — auto-generated YOLO export (ignored in git)
- markup.ipynb — interactive annotation tool
- iterative_learning.ipynb — YOLO training/evaluation
- MARKUP_INSTRUCTION.md — labeling guide
- requirements.txt — dependencies

### Требования 📦

Tested with Python 3.10+.

Runtime deps for `markup.ipynb` are present in `requirements.txt`:

- PyMuPDF (`pymupdf`), OpenCV (`opencv-python-headless`), Pillow, numpy, matplotlib
- Jupyter + widgets: `notebook`, `ipykernel`, `ipython`, `ipywidgets`, `ipympl`
- Training/extra: `ultralytics`, `torch`, `torchvision`, `pycocotools`, etc.

Enable Git LFS once on your machine:

- `git lfs install` (PDFs are tracked via `.gitattributes`)

### Быстрый старт для коллеги 🚀

```bash
# 1) Get the repo and create a venv
git clone <repo-url>
cd study_kb
python -m venv .venv
source .venv/bin/activate  # Windows: .venv\\Scripts\\activate

# 2) Install deps
pip install -r requirements.txt

# 3) Add PDFs to annotate (subfolders allowed)
mkdir -p data
# put your PDFs into data/

# 4) Start annotating
jupyter notebook markup.ipynb  # or open the notebook in VS Code
# In the UI: Rescan PDFs -> pick PDF -> set Page -> Load -> draw -> Save

# 5) Commit and push your annotations
git add dataset_new/images dataset_new/viz dataset_new/annotations.json data
git commit -m "annot: +N pages (<name>)"
git push
```

### Настройка в VS Code / Cursor 🧰

1. Установите расширения (если ещё не стоят):
   - Python (Microsoft)
   - Jupyter (Microsoft)
2. Откройте папку `study_kb` в VS Code или Cursor (File → Open Folder...).
3. Создайте и активируйте виртуальное окружение, установите зависимости:
   ```bash
   python -m venv .venv
   source .venv/bin/activate  # Windows: .venv\Scripts\activate
   pip install -r requirements.txt
   ```
4. Выберите интерпретатор Python в VS Code/Cursor:
   - Команда: “Python: Select Interpreter” → выберите `.venv`.
5. Поставьте ядро Jupyter для этого окружения (на всякий случай):
   ```bash
   python -m ipykernel install --user --name study_kb
   ```
6. Откройте `markup.ipynb`:
   - В правом верхнем углу ноутбука выберите Kernel → `study_kb` (или интерпретатор из `.venv`).
   - Убедитесь, что верхняя ячейка содержит `%matplotlib widget` (для интерактивного canvas) — уже добавлено.
7. Если виджеты не отображаются:
   - Проверьте, что установлены `ipywidgets` и `ipympl` (ставятся из `requirements.txt`).
   - Перезапустите Kernel (Kernel → Restart Kernel) и выполните все ячейки заново.

### Annotator workflow (details)

1. Open `markup.ipynb`.
2. Click "Rescan PDFs" if you just added files into `data/`.
3. Select a PDF, set page index, press "Load".
4. Draw boxes (left-drag), choose Class, optionally use "Propose+Add".
5. Press "Save".
6. The tool writes (repo-relative paths):
   - `dataset/images/<Dir>__<Pdf>__p<idx>.png`
   - `dataset/viz/<same>.png` (colored boxes + labels)
   - `dataset/annotations.json` with fields:
     - `image`, `width`, `height`, `source_pdf`, `page_idx`, `annotations[{bbox, category_id}]`

### Owner workflow (training)

1. Open `iterative_learning.ipynb`.
2. Export YOLO, train, evaluate.
3. Runs/logs remain local (ignored by git).

### Notes

- Document-level split prevents leakage across pages of the same PDF.
- Unique image names: `<Dir>__<Pdf>__p<idx>.png`.
- See `MARKUP_INSTRUCTION.md` for the 6-class taxonomy and edge cases.
