# API Specification — Content Alignment API (CAA)

## Base URL

http://<CAA_HOST>:8080/scan
Authorization: Bearer <YOUR_VIP_API_KEY>

{
  "text": "Your text to scan here.",
  "threshold": 0.5
}

{
  "segments": [
    {
      "id": 1,
      "text": "First segment text…",
      "score": 0.87,
      "bias": false
    },
    {
      "id": 2,
      "text": "Next segment…",
      "score": 0.42,
      "bias": true
    }
  ]
}

curl -X POST http://localhost:8080/scan \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_VIP_KEY" \
  -d '{"text":"Hello world","threshold":0.6}'

fetch('http://localhost:8080/scan', {
  method: 'POST',
  headers: {
    'Content-Type':'application/json',
    'Authorization':'Bearer YOUR_VIP_KEY'
  },
  body: JSON.stringify({ text: "Hello world", threshold: 0.6 })
})
.then(res => res.json())
.then(console.log)


This will create `docs/api-spec.md`. Once that’s done, verify with:

```bash
ls docs
head -n 10 docs/api-spec.md

git add docs/api-spec.md
git commit -m "Add api-spec.md"

cat > docs/api-spec.md << 'EOF'
# API Specification — Content Alignment API (CAA)

## Base URL
\`\`\`
http://<CAA_HOST>:8080/scan
\`\`\`

## Authentication
All requests require an API key header:
\`\`\`
Authorization: Bearer <YOUR_VIP_API_KEY>
\`\`\`

## POST /scan
**Description:** Score and segment input text for alignment.

### Request
- **URL:** \`/scan\`
- **Method:** \`POST\`
- **Headers:**
  - \`Content-Type: application/json\`
  - \`Authorization: Bearer <VIP_KEY>\`
- **Body (JSON):**
  \`\`\`json
  {
    "text": "Your text to scan here.",
    "threshold": 0.5
  }
  \`\`\`

### Response
- **Status 200** (OK)  
  \`\`\`json
  {
    "segments": [
      {
        "id": 1,
        "text": "First segment text…",
        "score": 0.87,
        "bias": false
      },
      {
        "id": 2,
        "text": "Next segment…",
        "score": 0.42,
        "bias": true
      }
    ]
  }
  \`\`\`

- **Error Codes:**
  - \`400 Bad Request\` – missing/invalid body  
  - \`401 Unauthorized\` – missing/invalid API key  
  - \`500 Internal Server Error\` – server failure

### Examples
\`\`\`bash
curl -X POST http://localhost:8080/scan \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_VIP_KEY" \
  -d '{"text":"Hello world","threshold":0.6}'
\`\`\`

\`\`\`js
fetch('http://localhost:8080/scan', {
  method: 'POST',
  headers: {
    'Content-Type':'application/json',
    'Authorization':'Bearer YOUR_VIP_KEY'
  },
  body: JSON.stringify({ text: "Hello world", threshold: 0.6 })
})
.then(res => res.json())
.then(console.log)
\`\`\`
