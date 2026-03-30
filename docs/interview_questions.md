# Part 3 Interview Questions

## Question 1

**Question:** Walk me through the three gates in an LSTM and explain how they help with long-range dependencies.

**Model Answer:** An LSTM uses a forget gate, input gate, and output gate to control information flow through the cell state. The forget gate decides what prior information to retain, the input gate decides what new information to write, and the output gate decides what to expose as the hidden state. This gating structure creates a more stable path for information and gradients across long sequences than a standard RNN.

## Question 2

**Question:** How do LSTMs improve on standard RNNs for sequence-to-sequence tasks?

**Model Answer:** Standard RNNs struggle with long-term dependencies because gradients shrink through many time steps. LSTMs improve this with gated memory that preserves relevant sequence information more reliably. In Seq2Seq settings, that makes it easier for the encoder or decoder to maintain context over longer inputs and outputs.

## Question 3

**Question:** Why did your team choose a chunk size of 220 words with 30 words of overlap, and what might break if you doubled it?

**Model Answer:** We chose a target chunk size of about 220 words with 30 words of overlap so the chunks stay within the assignment's 100-300 word window while still preserving enough context for technical explanations. The overlap helps adjacent chunks retain continuity across section boundaries. If we doubled the chunk size, more concepts would be mixed into each chunk, which could reduce retrieval precision, weaken source grounding, and make answers less specific.
