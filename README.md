# CineCompose: A Cinematographer’s Assistant to Compose a Framing Correctly (8-Way Move + Signed Rotation Regression)

**CineCompose** is an automated framing assistant designed to provide real-time, actionable camera-composition guidance. It predicts two kinds of spatial corrections: an **8-way movement suggestion** (plus a "no move" state) and a **signed rotation correction** to fix tilt or horizon errors. By leveraging a perturbation-based dataset pipeline, the system trains a multi-head transfer learning model without requiring manual labeling.

---

## 📌 Features
* **Dual-Correction Heads:** Provides simultaneous suggestions for 8-way directional adjustments (up, down, left, right, diagonals) and signed rotation tracking (clockwise/counterclockwise degrees).
* **Automated Data Pipeline:** Generates synthetic "bad" framing variants from professional stills using controlled directional crops and precise rotations, eliminating the need for expensive manual labeling.
* **Intelligent "No Suggestion" State:** A dedicated good-vs-bad classification head enables the assistant to stay quiet when an image is already well-composed.
* **Pretrained CNN Backbones:** Supports multiple architectures, with **EfficientNetB0** achieving the best overall performance.

---

## 📊 Evaluation & Results

Our final evaluation on a held-out test split highlights **EfficientNetB0** as the top-performing backbone:

| Metric | Performance |
| :--- | :--- |
| **Overall Test Loss** | 1.793 |
| **Move Accuracy** (Crop-only) | 0.456 |
| **Rotation MAE** (Rotate-only) | 4.20° |
| **Good/Bad Classification Accuracy** | 0.851 |

*Note: Training and validation logs indicate that overfitting typically begins after early epochs. This is primarily due to synthetic perturbation bias and inherent semantic ambiguity when defining a single "correct" adjustment for complex visual scenes.*

---

## ⚙️ Installation & Setup

1. **Clone the repository:**
   ```bash
   git clone https://github.com
   cd CineCompose
   ```

2. **Install dependencies:**
   ```bash
   pip install -r requirements.txt
   ```

3. **Generate the perturbed dataset:**
   Place your professional stills dataset in `data/raw/` and run:
   ```bash
   python data/generate_perturbations.py
   ```

4. **Train the multi-head model:**
   ```bash
   python models/train.py --backbone efficientnetb0 --epochs 10
   ```

---

## 📜 Citation
If you find this work useful in your research, please cite our paper:

```bibtex
@inproceedings{almahmud2026cinecompose,
  title={CineCompose: A Cinematographer’s Assistant to Compose a Framing Correctly (8-Way Move + Signed Rotation Regression)},
  author={Al-Mahmud and Labid, Reduan Nur and Molla, Md Mamun},
  booktitle={Multi-Task Transfer Learning for 8-Way Framing Correction and Signed Rotation Regression},
  institution={Department of CSE, BRAC University, Dhaka, Bangladesh},
  email={al.mahmud@g.bracu.ac.bd, reduan.nur.labid@g.bracu.ac.bd, md.mamun.molla@g.bracu.ac.bd},
  year={2026},
  keywords={Cinematography, Composition Assistance, Camera Framing, Multi-Task Learning, Transfer Learning, Rotation Regression, Data Augmentation}
}
```
