---
title: "High-Speed Tactile Braille Reading"
venue: "IEEE RA-L"
layout: page
description: Vision-based tactile sensing system for robotic Braille reading.
img: assets/img/publication_preview/braille_robot.png
importance: 1
category: work
tags: [robotics, tactile-sensing, perception, publication]
---

### Overview
Designed and built a high-speed vision-based tactile sensing system for robotic Braille reading, inspired by human sliding touch. The system achieves fast and accurate character recognition by leveraging continuous motion rather than static probing.

[High-speed tactile braille reading via biomimetic sliding interactions](https://doi.org/10.1109/LRA.2024.3356978) 
**Potdar, P., Hardman, D., Almanzor, E., Iida, F.**  
IEEE Robotics and Automation Letters (RA-L), 2024  

<div class="video-container">
  <iframe
    width="100%"
    height="400"
    src="https://www.youtube.com/embed/qrsXXobWy3Y"
    frameborder="0"
    allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
    allowfullscreen>
  </iframe>
</div>

### Key Contributions
- Developed a **biomimetic sliding interaction pipeline** for tactile perception
- Built a **real-time inference system** for sequential Braille character recognition:
    - **Deblurring Autoencoder** to remove motion blur from sliding touch images
    - **YOLOv8 Object Detection** to localise and classify Braille characters in the deblurred images
    - **Data-driven Error Correction** to improve accuracy by learning common misclassification patterns
- Demonstrated robust performance under motion and sensing noise  

### Results
- **315 words per minute** reading speed  
- **87.5% character-level accuracy**  
- Significant speed improvement over prior static tactile methods  

### Notes
This work highlights the importance of **active perception** and **motion-driven sensing** for efficient robotic interaction.