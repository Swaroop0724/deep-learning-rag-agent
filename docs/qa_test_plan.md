# QA Test Plan

## Integration Tests

| Test | Input | Expected Behavior | Status |
|---|---|---|---|
| Normal query | `Explain the vanishing gradient problem` | Relevant RNN or LSTM chunks retrieved, answer grounded in corpus, at least one source citation shown | Pass in retrieval smoke test; final UI citation capture still recommended |
| Off-topic query | `What is the capital of France?` | Hallucination guard fires, answer says relevant context was not found, no fabricated deep learning response | Pass in retrieval smoke test (`0` chunks returned) |
| Duplicate ingestion | Upload `rnn_intermediate.md` twice | First ingest adds chunks, second ingest reports duplicates skipped | Pass in smoke test (`9` ingested, then `9` skipped on re-ingest) |
| Empty query | Submit blank chat input | UI should not crash and should avoid sending an invalid query | Pass by code path review; `st.chat_input` blocks blank submission and retrieval returns `[]` for blank input |
| Cross-topic query | `How do LSTMs improve on RNNs for Seq2Seq tasks?` | Retrieval spans more than one concept area and answer synthesizes grounded context with citations | Pass in retrieval smoke test (returned LSTM, RNN, and Seq2Seq chunks) |

## Risk Assessment

| Risk | Why It Matters | Mitigation |
|---|---|---|
| Thin corpus coverage | Retrieval quality falls quickly when the corpus is too small or too generic | Add LSTM, Seq2Seq, Autoencoder, and at least one bonus topic |
| Environment drift | Chroma collections or local caches can break when embedding config changes | Reset `data/chroma_db` when changing embedding models or collection settings |
| Prompt drift | The model may answer from prior knowledge instead of retrieved context | Keep strict grounding instructions and verify citations during the demo |

## Manual Verification Notes

- Unit tests pass with `uv run pytest tests/ -v`
- Dependency import check passes with `uv run python -c "import chromadb; import langchain; import langgraph; print('All dependencies OK')"`
- Corpus chunking smoke test showed all authored markdown files producing 3 chunks each, with observed chunk sizes in the 118-143 word range
- Duplicate skipping has been verified in smoke testing with real corpus files
- Retrieval smoke testing verified on-topic, off-topic, and cross-topic behavior against a temporary Chroma collection
- Final UI walkthrough with visible citations should still be recorded for the submission video
