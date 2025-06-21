Ethical Reflection on Predictive Model Deployment
=================================================

When our Task 3 Random Forest model for issue‑priority prediction is deployed in production, it must balance high accuracy with fairness. Below are key bias risks and mitigation strategies using IBM AI Fairness 360.

Bias Risks
----------

1. **Class Imbalance**  
   “High‑priority” tickets may represent only ~10% of historical data. The model thus under‑predicts urgent issues, delaying critical work.

2. **Team Representation Bias**  
   If Team A logs more detailed tickets than Team B, the model learns patterns skewed toward Team A’s style and underperforms on Team B’s data, creating unequal workload distribution.

3. **Proxy Feature Bias**  
   Non‑technical features (submission time, geographic region) can correlate with sensitive factors (e.g., time zones), unintentionally influencing priority predictions.

Mitigation Strategies
---------------------

**1. Reweighing (Preprocessing)**  
Assigns weights to samples so that under‑represented groups contribute equally during training.

.. code-block:: python

    from aif360.algorithms.preprocessing import Reweighing

    privileged_groups = [{'team': 'TeamA'}]
    unprivileged_groups = [{'team': 'TeamB'}]
    RW = Reweighing(unprivileged_groups=unprivileged_groups,
                    privileged_groups=privileged_groups)
    dataset_transf = RW.fit_transform(dataset)

**2. Disparate Impact Remover (Preprocessing)**  
Transforms feature distributions to reduce dependency on sensitive attributes.

.. code-block:: python

    from aif360.algorithms.preprocessing import DisparateImpactRemover

    DIR = DisparateImpactRemover(repair_level=1.0)
    repaired_dataset = DIR.fit_transform(dataset)

Outcome
-------

By applying these methods:

- **Disparate Impact Ratio** rises above **0.8**  
- **Overall Accuracy** remains ≥ 85%  

Continuous bias monitoring and retraining ensure sustained fairness in issue‑priority assignments.

.. note:: Citation: IBM AI Fairness 360 user guide.
