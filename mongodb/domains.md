```json
{
  "_id": {
    "$oid": "66d903916c8b1f3a173908e8"
  },
  "domain": "https://thehackernews.com",
  "domain_name": "thehackernews",
  "domain_paths": [
    {
      "path": [
        "/search/label/Cyber%20Attack",
        "/search/label/Vulnerability",
        "/search/label/data%20breach"
      ],
      "crawling_command": {
        "article_cell": [
          {
            "name": "a",
            "class": "story-link"
          }
        ],
        "title": [
          {
            "name": "h2",
            "class": "home-title"
          },
          {
            "get": "text"
          }
        ],
        "date": [
          {
            "regex": "[A-Z][a-z]{2}\\s\\d{1,2},\\s\\d{4}"
          },
          {
            "get": "text"
          }
        ],
        "url": [
          {
            "get": "href"
          }
        ]
      }
    }
  ]
}
```