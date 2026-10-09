# Skeleton-Based Yoga Pose Correction

**Ongoing research project | Current stage: research design**

Investigating predictive and generative deep-learning approaches to transform imperfect yoga-pose skeletons into corrected keypoint configurations, followed by interpretable joint-angle feedback.

**Author:** [Guruvignesh R M](https://github.com/guruvignesh-tech)

## Research objective

Given an observed skeleton and the intended yoga pose, learn a plausible corrected configuration using 17 body keypoints while preserving body proportions and valid pose variation. Compare observed and predicted joint angles to identify deviations and provide targeted feedback. The intended pose will initially be supplied as a class label.

## Current progress

- [x] Defined the skeleton-to-skeleton correction problem.
- [x] Designed the proposed 17-keypoint research pipeline.
- [x] Outlined baseline comparisons and quantitative evaluation.
- [ ] Inspect Yoga-82 keypoint files, labels, coordinate conventions, and reference quality.
- [ ] Implement and visually verify landmark mapping and normalization.
- [ ] Generate controlled synthetic posture errors.
- [ ] Train a residual regression baseline.
- [ ] Compare predictive/generative architectures.
- [ ] Validate joint-level feedback on reviewed real posture errors.

This repository currently documents the research design. Model implementation, trained weights, benchmarks, and a working demonstration are pending. No experimental performance or novelty claim is made at this stage.

## Proposed pipeline

```text
Yoga-82 reference skeletons
    -> quality checks and 17-keypoint mapping
    -> normalization and leakage-aware splits
    -> controlled posture perturbations
    -> learned correction model
    -> predicted corrected skeleton
    -> joint-angle comparison
    -> threshold-based corrective feedback
```

Synthetic perturbations are used for training and controlled evaluation. At deployment, the input would be the user's observed skeleton.

```text
predicted_correction = model(observed_skeleton, intended_pose)
corrected_skeleton = observed_skeleton + predicted_correction
```

## Planned model investigation

1. **Unchanged-input baseline:** measure improvement over doing nothing.
2. **Residual MLP:** establish a simple learned baseline.
3. **Skeleton Transformer:** investigate relationships between joint tokens.
4. **Conditional variational or graph-based model:** explore if justified by baseline results and related work.

Candidate losses include coordinate reconstruction, joint-angle error, and bone-length consistency. Architecture and loss choices remain experimental.

## Data and evaluation

The planned source is Yoga-82 keypoint data referenced by Yoga-MAtNODE. Actual files and extraction conventions still require validation. Pose-class labels do not certify expert-correct posture; reference skeletons must be quality-screened.

Planned metrics:

- Keypoint reconstruction error with explicit coordinate units and normalization.
- Percentage of Correct Keypoints (PCK), with a documented threshold and scale.
- Joint-angle deviation in degrees over defined joint triplets.
- Improvement over uncorrected inputs and false corrections on valid poses.
- Joint-level feedback precision and recall when reviewed annotations are available.

Split original samples before creating perturbed variants. Check duplicates and subject/source overlap where metadata permits. Report synthetic reconstruction separately from real-error correction.

## Experimental controls and next steps

- Verify landmark ordering and whether coordinates are image-normalized or estimated world coordinates.
- Visually check mapping and skeletons against source images where available.
- Generate joint-angle and kinematic-chain errors that preserve bone lengths. Include unchanged references.
- Evaluate missing-joint recovery and detector noise separately from posture correction.
- Use identical splits for baseline comparisons; record seeds, configurations, and checkpoint selection criteria.
- Select hyperparameters and feedback thresholds on validation data; reserve the test set for evaluation.
- Compare coordinate-only training against angle and bone-consistency losses.
- Test held-out perturbation types and severities.
- Validate practical feedback on a separate reviewed collection of real posture errors.

Synthetic reconstruction alone supports a limited proof of concept. Establishing a research contribution requires related-work analysis and completed experiments.

## Relationship to prior work

This project follows a research direction suggested by **Rashi Niyas P**: investigate learned predictive/generative correction of yoga skeletons and evaluate joint-angle feedback.

[Yoga-MAtNODE](https://github.com/rashiniyasp/Yoga-MAtNODE) is the reference work for skeleton-based yoga pose recognition and related data/preprocessing resources. This repository documents a separate correction investigation. Classification results from the reference work are not results of this project.

## References

- Rashi Niyas P et al., [Yoga-MAtNODE repository](https://github.com/rashiniyasp/Yoga-MAtNODE).
- Manisha Verma et al., [Yoga-82: A New Dataset for Fine-Grained Classification of Human Poses](https://openaccess.thecvf.com/content_CVPRW_2020/html/w70/Verma_Yoga-82_A_New_Dataset_for_Fine-Grained_Classification_of_Human_Poses_CVPRW_2020_paper.html), CVPR Workshops, 2020.
- [Yoga-82 dataset project page](https://sites.google.com/view/yoga-82/home).
- [MediaPipe Pose Landmarker documentation](https://developers.google.com/edge/mediapipe/solutions/vision/pose_landmarker/python).

Dataset usage follows the original source's terms. Images, datasets, and third-party code are not redistributed here.
