# Gate Logic — Mirror of Discernment Truth Gates

Each of the 10 Truth Gates applies a specific heuristic or check to filtered segments.  
Below is pseudocode and tuning guidance for each.

---

## Gate 1: Relevance  
**Beat 2** — ensure the segment matches user-selected topics.

```python
def gate_relevance(segment_text, user_topics):
    tokens = tokenize(segment_text)
    if any(topic in tokens for topic in user_topics):
        return True
    return False

def gate_prototype(segment_text, archetype_model):
    pred = archetype_model.predict(segment_text)
    return pred  # returns archetype label

def gate_fact(segment_text, fact_api_client):
    result = fact_api_client.check(segment_text)
    return result.is_verified  # True if verified

def gate_consistency(segment_text):
    sentences = split_sentences(segment_text)
    for s1, s2 in combinations(sentences, 2):
        if contradicts(s1, s2):
            return False
    return True

def gate_emotion(segment_text, sentiment_model):
    score = sentiment_model.polarity(segment_text)
    return abs(score) < 0.7  # flag overly emotional

def gate_authority(source_url, whitelist, blacklist):
    domain = extract_domain(source_url)
    if domain in whitelist: return True
    if domain in blacklist: return False
    return None  # unknown

def gate_novelty(segment_text, recent_cache):
    sim = max(similarity(segment_text, past) for past in recent_cache)
    return sim < 0.8  # novel if <80% similar

def gate_representation(segment_text):
    entities = extract_entities(segment_text)
    return len(set(entities)) >= 2  # two or more viewpoints

def gate_coherence(segment_text, coherence_model):
    score = coherence_model.score(segment_text)
    return score > 0.5

def gate_integration(gate_results):
    return sum(gate_results) >= 8


Once that’s done, verify & commit:

```bash
ls docs
head -n 10 docs/gate-logic.md
git add docs/gate-logic.md
git commit -m "Add gate-logic.md spec"
