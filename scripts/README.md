# Scripts

Stable user-facing commands stay at the top level:

- `analyze_quality.py`: score trajectory quality.
- `evaluate_task_outcomes.py`: extract keyframes and evaluate task outcomes with Qwen-VL.
- `visualize_task_outcomes.py`: build an HTML dashboard from a saved task-outcome summary.
- `extract_keyframes.py`: extract behavior keyframes.
- `analyze_monitor_stream.py`: process an online-style JSONL stream and save event results.
- `replay_monitor_stream.py`: replay a monitor stream in a browser UI.

Supporting commands live under `tools/`:

- `export_online_stream.py`: convert offline episodes to replayable JSONL streams.
- `inspect_episode_camera.py`: inspect automatic camera and moving-arm selection.
- `review_episodes.py`: rank and manually review suspicious episodes.
- `inspect_agibot_action_state.py`: inspect raw AgiBot parquet action/state behavior.
- `viz_lerobot.sh`: launch the LeRobot dataset visualizer from the registry.

## Data Visualization

Use `tools/viz_lerobot.sh` to visualize a registered episode with LeRobot and
Rerun. The script reads `repo_id` and `root` from the repository's
`datasets.yaml`, so only the dataset name and episode index are required:

```fish
# List registered datasets.
bash scripts/tools/viz_lerobot.sh list

# Visualize episode 0 from each STEA example dataset.
env PATH=/data/xiuchao/cache/conda/envs/openpi/bin:$PATH \
	bash scripts/tools/viz_lerobot.sh STEA_pick_success 0

env PATH=/data/xiuchao/cache/conda/envs/openpi/bin:$PATH \
	bash scripts/tools/viz_lerobot.sh STEA_pick_failure 0
```

The `openpi` environment is used here because it provides the
`lerobot-dataset-viz` command and a compatible Rerun installation. If that
environment is already activated, omit the `env PATH=...` prefix.

To save a visualization as a Rerun recording without opening the viewer:

```fish
env PATH=/data/xiuchao/cache/conda/envs/openpi/bin:$PATH \
	bash scripts/tools/viz_lerobot.sh STEA_pick_success 0 \
	--save 1 --output-dir outputs/visualization
```

The output is an `.rrd` file that can be opened later with the `rerun` command
from the same environment.

Saved ad hoc commands and historical experiments live under `experiments/`.
Reusable behavior belongs in `dataset_io/`, `trajectory/`, `robehavior/`,
`quality/`, or `workflows/`; Python code must not import from `scripts/`.