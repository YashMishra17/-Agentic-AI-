#Part A – Small RAG design (concrete, not generic) 
RAG Design (Fully Humanized)

#High-Level Decisions and Reasoning

For the long documents, I’d split them into chunks of roughly 512–800 words, making sure there’s a small overlap between chunks. This is to prevent any piece of information from being cut in half, which could cause the system to miss a fact.

Short FAQs and product tables are small enough that splitting doesn’t help. I’d leave these as whole units so each entry remains self-contained.

For the CSV product list, I’d treat each row as a separate chunk and store metadata like product ID, name, ingredients, and contraindications. This lets us directly cite the source for each fact, without ambiguity.

For retrieval, I’d use a combination of semantic embeddings and exact-match keyword search. Embeddings let the system understand variations in phrasing, like “supports digestion” vs. “aids gut comfort,” while exact-match search ensures we catch precise product names or ingredient terms.

When a user asks a question, I’d retrieve about 8 candidate chunks, then rerank them and pass the top 4 to the model. This gives enough context for a full answer but avoids overloading the model.

The prompt to the AI would be clear: only answer using the retrieved content. If you don’t know, say so. Always include a reference to the source, like “(products_catalog.csv::KA-P001).”

After the AI generates an answer, I’d check every statement against the retrieved chunks. Anything that can’t be traced back gets removed, and the system would fallback to “I don’t know based on the provided content.”

I’d log the user query, retrieved chunks, the generated answer, and the citation check results. This creates a record for auditing and helps editors understand why each fact is present.

To keep things manageable and fast, the total text passed to the AI would stay under about 2,500 tokens. This balances completeness and efficiency.
