GoodNotes-AI (study_kb)

This folder can serve as a standalone repository for layout annotation and training.

Structure
- data/ — place source PDFs here (subfolders allowed)
- dataset_new/
  - images/ — rendered pages for annotated samples
  - viz/ — rendered previews with drawn bounding boxes for QA
  - annotations.json — labels with metadata
  - yolo_iter/ — auto-generated YOLO export (ignored in git)
- markup.ipynb — interactive annotation tool
- iterative_learning.ipynb — YOLO training/evaluation
- MARKUP_INSTRUCTION.md — labeling guide
- requirements.txt — dependencies

Setup
- Install: pip install -r requirements.txt
- Enable Git LFS: git lfs install (PDFs tracked via .gitattributes)

Annotator workflow (for collaborators)
1) Get the repo
   - git clone <repo-url>
   - cd study_kb
   - pip install -r requirements.txt
2) Add PDFs to annotate
   - Put files into data/ (you may organize subfolders)
   - git lfs track "*.pdf" is already configured
3) Annotate
   - Open markup.ipynb
   - Use “Next ✨” to jump to the next unannotated page
   - Draw boxes (left mouse drag), pick class, Save
   - The tool saves:
     - dataset_new/images/<Dir>__<Pdf>__p<idx>.png
     - dataset_new/viz/<same>.png (overlays for QA)
     - dataset_new/annotations.json (appends/updates record with source_pdf, page_idx, annotator)
4) Commit and push your annotations
   - git add dataset_new/images dataset_new/viz dataset_new/annotations.json data
   - git commit -m "annot: +N pages (your_name)"
   - git push

Owner workflow (training)
- Open iterative_learning.ipynb
- Export YOLO, train, evaluate
- Runs/logs are ignored from git (kept local)

Notes
- Document-level split prevents leakage across pages of the same PDF
- Unique image names: <Dir>__<Pdf>__p<idx>.png
- See MARKUP_INSTRUCTION.md for the 6-class taxonomy and edge cases
