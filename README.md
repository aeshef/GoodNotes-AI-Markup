GoodNotes-AI (study_kb)

This folder can serve as a standalone repository for layout annotation and training.

Structure
- data/ — place source PDFs here (subfolders allowed)
- dataset_new/
  - images/ — rendered pages for annotated samples
  - annotations.json — labels with metadata
  - yolo_iter/ — auto-generated YOLO export
- markup.ipynb — interactive annotation tool
- iterative_learning.ipynb — YOLO training/evaluation
- MARKUP_INSTRUCTION.md — labeling guide
- requirements.txt — dependencies

Setup
- pip install -r requirements.txt
- git lfs install (PDFs tracked via .gitattributes)

Workflow
- Add PDFs into data/
- Open markup.ipynb, annotate, Save (adds/updates annotations.json and images/)
- Train in iterative_learning.ipynb; monitor metrics and class balance

Notes
- Document-level split prevents leakage across pages of the same PDF
- Unique image names: <Dir>__<Pdf>__p<idx>.png
