```json
{
  "_id": {
    "$oid": "식별용 ID"
  },
  "domain": "도메인 루트 페이지",
  "domain_name": "도메인 네임",
  "domain_paths": [
    {
      "path": [
        "크롤링 가능한 URL 경로 1",
        "크롤링 가능한 URL 경로 2",
        "크롤링 가능한 URL 경로 3"
      ],
      "crawling_command": {
        "article_cell": [
          {
            "name": "태그 이름",
            "class": "클래스 이름"
          }
        ],
        "title": [
          {
            "name": "태그 이름",
            "class": "클래스 이름"
          },
          {
            "get": "텍스트 위치"
          }
        ],
        "date": [
          {
            "regex": "정규표현식"
          },
          {
            "get": "텍스트 위치"
          }
        ],
        "url": [
          {
            "get": 텍스트 위치
          }
        ]
      }
    }
  ]
}
```