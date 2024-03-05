# How to Convert Your DeepLabCut Project Between TensorFlow and PyTorch🔦

Switching between TensorFlow and PyTorch in DeepLabCut unlocks the potential to capitalize on the unique advantages offered by these leading machine learning frameworks. This guide is tailored for users aiming to transition an existing DeepLabCut project to a different backend.

## Preparing for Conversion

### (1) Update Your `config.yaml`

To modify your `config.yaml` file, you have two convenient options: using a text editor/Python IDE or leveraging the DeepLabCut GUI.

### For TensorFlow to PyTorch Conversion

To switch your project's engine to PyTorch, add the following line after the `task`, `scorer`, `date`, `project_path` entries in your `config.yaml`:

```yaml
# Default DeepLabCut engine to use for shuffle creation (either pytorch or tensorflow)
engine: pytorch
```

This adjustment in your `config.yaml` designates PyTorch as the chosen framework for model training and inference within your project.

### For PyTorch to TensorFlow Conversion

If transitioning from a PyTorch-based setup back to TensorFlow, you'll need to modify the `engine` parameter as follows:

```yaml
# Default DeepLabCut engine to use for shuffle creation (either pytorch or tensorflow)
engine: tensorflow
```
For those who prefer a graphical interface, DeepLabCut's GUI offers an intuitive way to edit your `config.yaml`:

1. Launch the DeepLabCut GUI by navigating to your project's environment and executing `python -m deeplabcut`.
2. Once the GUI is open, locate and click on the "Edit Config.yaml" button⏺. This action opens the `config.yaml` file within the GUI, allowing for easy modifications.
3. After making your changes (for example, updating the `engine` parameter to `'pytorch'` or `'tensorflow'`), click "Save" to apply your edits.

**Important:** After editing the `config.yaml` file through the GUI, you must restart the GUI to ensure all changes take effect. This step is crucial for the updated configurations to be recognized by DeepLabCut during your session.

```yaml
# Project definitions (do not edit)
Task: mouse
scorer: deeplabcut
date: Feb26
multianimalproject: false
identity:

# Project path (change when moving around)
project_path: /Users/DeepLabCut/Desktop/mouse

# Default DeepLabCut engine to use for shuffle creation (either pytorch or tensorflow)
engine: pytorch
```

### (3) Save and Validate `config.yaml`

Upon updating your `config.yaml`, save the file and thoroughly check for any potential spacing or syntax errors, as YAML files are particularly sensitive to such issues.

### Re-generate the Training Dataset

When shifting frameworks, it's imperative to recreate your training dataset to fit the requirements of the newly selected backend. DeepLabCut facilitates this with diverse model options available for each framework, selectable through the `net_type` parameter.

#### Transitioning to PyTorch

```python
deeplabcut.create_training_dataset(config_path, net_type='resnet50')
```

#### Reverting to TensorFlow

```python
deeplabcut.create_training_dataset(config_path, net_type='resnet_50')
```

*Note:* Replace `config_path` with the path to your `config.yaml` file. The choice of `net_type` depends on the model pool associated with your selected framework. For detailed information on available models and additional guidance, refer to the [DeepLabCut documentation](https://deeplabcut.github.io/DeepLabCut/docs/standardDeepLabCut_UserGuide.html).

### (4) Verify Data Integrity

To ensure a flawless transition of your labeled data to the new backend, use the `check_labels` command:

```python
deeplabcut.check_labels(config_path)
```

### (5) Training

With your project aligned to the new framework, you're ready to commence training. Execute DeepLabCut's standard training command, and the specified backend in your `config.yaml` will be employed:

```python
deeplabcut.train_network(config_path)
```

## Advanced Considerations

Transitioning between TensorFlow and PyTorch may impact your project's performance, training speed, and accuracy. It's advisable to monitor the training process attentively and adjust hyperparameters as necessary. Each backend introduces its optimizations and features, potentially affecting your project's results.





