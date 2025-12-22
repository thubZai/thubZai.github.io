---
layout: single
author_profile: true
---

## Random Musings

### A somewhat organized dump of thoughts and questions I wrestle with!

>### the human baseline

 Human cognition includes biases, confabulation, and limited working memory. We don't want to replicate these flaws. The goal isn't "think like humans", but **to think helpfully in human contexts,** with our adaptability but fewer of our failure modes.

**cognition & world models**

- Models need direct interaction loops with environments (physical or simulated), not just static data. This builds intuition about causality, physics, and consequences.
- Beyond pattern matching, how can world models be rich enough to include internal simulations that represent objects, time, and hidden states? Think of "mental models" that can be manipulated for planning.
- These models must include uncertainty estimates and anomaly detection to detect when they're outside their training distribution.

**temporal architecture & active memory**

- Explicit time representations (not just positional encodings). Systems need to know when events occurred, their duration, and the deadline pressure.
- They need **working memory + long-term memory,** meaning, a differentiable scratchpad for immediate context plus a stable, updatable knowledge store. This may help prevent catastrophic forgetting and enable cumulative learning.
- Memory should be auditable and forgettable, like the ability to scrub unsafe or outdated knowledge, and to log reasoning traces for alignment checks.

**continual & meta-learning**

- We need online learning without drift. Something like training on-the-fly while avoiding catastrophic forgetting. Techniques like elastic weight consolidation or replay buffers help, but we need better solutions.
- How can they learn from mistakes, detect their own errors, generate counterfactuals (like what if I'd done X?), and update policies?
- But the updates must be bounded and reversible. We can't let a single mistake rewrite core safety goals….

**context-awareness & self-modelling**

- The model should know its capabilities, limitations, and current objectives. This is key to know when to defer to humans or seek clarification.
- We don't just need more tokens, but active attention management, deciding what's relevant now vs later and pruning irrelevant info.

**hierarchical planning & goal alignment**

- Fast reactive loops + slow deliberative planning. We don't think through every muscle movement, we operate at abstraction layers.
- With this, we should also prioritize verifiable goal alignment and methods to ensure the system won't optimize for proxy measures at the expense of true intent.

>### VLA models and embodied agents

Generalization and efficiency are key. [Recent work](https://arxiv.org/pdf/2403.10967) on contextual world models shows promise - by explicitly modeling context variables (like robot mass), DreamerV3-style agents can zero-shot generalize to new settings. An open question is whether VLA agents can similarly factor in context (eg, environment layout or object properties) to adapt on the fly. π₀ introduces a ["flow" model](https://arxiv.org/pdf/2410.24164) on top of a pretrained VLM, training on diverse robots (arms, mobile bases) to achieve tasks in one shot. Can we extend this to even wider settings (quadrupeds, drones, factory arms)?

- How can an embodied agent autonomously invent learning tasks, as in open-ended or curiosity-driven learning? For eg, an agent in a kitchen might invent tasks of gradually increasing complexity (first "point at object," later "cook a meal"). Designing reward or imitation signals for such self-generated curricula is largely unexplored.
- Building on [RT-2](https://robotics-transformer2.github.io/), how can we train one model on heterogeneous data (videos, images, text, and multi-robot demos) so it can control any robot? [π₀](https://arxiv.org/pdf/2410.24164) uses flow matching on a VLM to gain internet knowledge for dexterous tasks. Can we scale this to even more modalities (like audio sensors, haptic inputs) or to real-time model-predictive control?
- What mechanisms allow a VLA model to follow novel instructions or handle unseen tools? [RT-2](https://robotics-transformer2.github.io/) saw its model interpret instructions about "place object on icon" it never saw in training. Understanding and improving such transfer (perhaps via better world models or modular policy components) could be crucial.

>### reinforcement learning & robotics

- Given the high cost and complexity of collecting real-world robot interaction data, how can VLA models learn efficiently from limited data, use synthetic data effectively, or utilize self-supervised and unsupervised learning techniques to reduce reliance on extensive human-annotated datasets?
- It would be sick to develop capabilities for long-horizon planning (break down high-level goals into executable sub-tasks and recover from errors during execution) nd execute multi-step tasks that require sequential decision-making and interaction with the environment over extended periods….
- How can we make agents learn policies that generalize effectively to unseen environments/tasks, nd can that knowledge be efficiently transferred between different domains to accelerate learning and reduce the need for extensive retraining?

>### beyond pattern recognition

- How can we make a model break down a complex visual scene & its accompanying text into fundamental, disentangled concepts? like (not just "dog" and "ball," but the action of "fetching," the spatial relation "under," the attribute "fluffy").

The real magic is when a model can take these core components nd understand or generate descriptions for entirely new scenarios it hasn't explicitly been trained on.

```python
# imagine a model that learns separate 'slots' for objects, actions, attributes, relations

class DisentangledConcepts:
    def __init__(self):
        self.objects = []  # eg ['cat', 'table', 'lamp']
        self.actions = []  # eg ['sitting_on', 'looking_at']
        self.attributes = {'cat': ['black', 'sleepy']}
        self.spatial_relations = {'cat': ('on', 'table')}
    
    def describe_scene(self):
        # combine these to generate or understand: "a sleepy black cat is sitting on the table"
        # or even reason about a novel scene - "what if the lamp (object) fell (action) near the cat?"
        pass

# how can we encourage transformers to learn such structured representations?
# maybe through structured attention mechanisms or auxiliary losses?
```
- Instead of letting a network figure out spatial relationships on its own from scratch, how can we directly teach it a "concept" of left, right, and under? And by doing so, will it become significantly better at connecting words to the correct objects in an image/video?

- Can we design a transformer-based VLM that internally represents concepts like objects, their attributes, and their actions as separate, modular "slots"? nd what if we train these slots with a specific contrastive objective that emphasizes matching attributes and actions, will the model become significantly better at zero-shot composition?

>### mechanistic interpretability
- If a model says an image shows "a sign of an impending cosmic event", is it looking at a weird cloud formation, a lens flare it mistook for a supernova, or did it actually learn something subtle from the correlated text about whatever it saw?

- Attention visualization, still a go-to, but often not enough. We need to go deeper than just "where the model looked."

- Can we get the model to explain its reasoning in terms of the core abstractions it has learned?, like "I see features X, Y, Z which correspond to my 'nebula' concept, and the text mentions 'star formation'.

- What if I change a small part of the image or text? How does the model's understanding shift?

- If we identify a flawed concept the model has learned like (it thinks all birds can fly), can we perform a targeted "surgery" on its representations to correct this without retraining the entire thing?

>### efficiency & alignment
Our brains are incredibly efficient, but models, not so much. This is where inspiration from things like UnSloth and efficient attention mechanisms becomes crucial.

- Can we selectively quantize parts of the model less critical for high-level reasoning?

- Techniques like multi-query attention or shared key-value layers (I've seen it in Character AI's work) are vital for reducing the footprint of large multimodal models.

- torch.compile is a blessing! fusing operations like RMS Layernorm, RoPE embeddings, and SwiGLU reduces memory bandwidth, which is often the bottleneck. [this one is a good reference](https://arxiv.org/html/2410.10989v2).

- How easily can a model be fooled by subtle image manipulations paired with misleading text?

- Multimodal datasets can inherit and amplify societal biases, but where is the bias encoded?




