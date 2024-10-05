---
# Leave the homepage title empty to use the site title
title: ""
date: 2022-10-24
type: landing

design:
  # Default section spacing
  spacing: "6rem"

sections:
  - block: resume-biography-3
    content:
      # Choose a user profile to display (a folder name within `content/authors/`)
      username: admin
      text: ""
      # Show a call-to-action button under your biography? (optional)
      button:
        text: Download CV
        url: ../uploads/resume.pdf
    design:
      css_class: dark
      background:
        color: black
        image:
          # Add your image background to `assets/media/`.
          filename: backkground.jpg
          filters:
            brightness: 1.0
          size: cover
          position: center
          parallax: false
  - block: markdown
    content:
      title: '📚 This is my Blog'
      subtitle: ''
      text: |-
        hello! This blog contains my project records, my external activity records, my reading records, and my hobbies records. Please come visit us often! thank you 😃
    design:
      columns: '1'
  - block: collection
    id: works
    content:
      title: My projects
      filters:
        folders:
          - publication
        featured_only: true
    design:
      view: article-grid
      columns: 3

  - block: collection
    id: talks
    content:
      title: Talk about Extracurricular Activities
      filters:
        folders:
          - event
    design:
      view: article-grid
      columns: 3

  - block: collection
    id: news
    content:
      title: News about my hobby
      filters:
        folders:
          - post
    design:
      view: article-grid
      columns: 3

  - block: slider
    content:
      slides:

      - title: <span style="font-size:90%">AI</span>
        content: <span style="font-size:90%">의료/항공우주/컨텐츠 등 특성화 분야에 적용 가능한 AI 기술 개발<span style="font-size:90%">
        align: center
        background:
          image:
            filename: Ai.jpg
            filters:
              brightness: 0.4
          position: center
          color: '#000'

      - title: <span style="font-size:90%">Medical AI</span>
        content: <span style="font-size:90%">의료AI를 통한 질병 진단 및 환경 개선</span>
        align: center
        background:
          image:
            filename: medical.jpg
            filters:
              brightness: 0.4
          position: center
          color: '#000'

      - title: <span style="font-size:90%">Healthcare</span>
        content: <span style="font-size:90%">의료 및 헬스케어 분야에 적용 가능한 AI 기술 개발</span>
        align: center
        background:
          image:
            filename: healthcare.jpg
            filters:
              brightness: 0.4
          position: center
          color: '#000'

      - title: <span style="font-size:90%">Aerospace</span>
        content: <span style="font-size:90%">항공우주에 적용 가능한 특성화 AI 기술 개발</span>
        align: center
        background:
          image:
            filename: aerospace.jpg
            filters:
              brightness: 0.4
          position: center
          color: '#000'

      - title: <span style="font-size:90%">Contents AI</span>
        content: <span style="font-size:90%">웹툰 및 컨텐츠 적용 가능한 특성화 AI 기술 개발</span>
        align: center
        background:
          image:
            filename: contents.jpg
            filters:
              brightness: 0.4
          position: center
          color: '#000'

      - title: <span style="font-size:90%">Mathematics</span>
        content: <span style="font-size:90%">AI와 관련된 수학 및 최적화 이론 연구</span>
        align: center
        background:
          image:
            filename: mathematics.jpg
            filters:
              brightness: 0.4
          position: center
          color: '#000'

      - title: <span style="font-size:90%">Development</span>
        content: <span style="font-size:90%">기반 기술을 활용한 Full-Stack 어플리케이션 개발</span>
        align: center
        background:
          image:
            filename: development.jpg
            filters:
              brightness: 0.4
          position: center
          color: '#000'





  - block: community/slider
    content:
      title: Reading
      slides:
      - background: 
          content: 마음사전, 김소연
          image:
            filename: mainslider/heartdic.jpg
      - background: 
          content: 프로테스탄트윤리와자본주의정신, 막스 베버
          image:
            filename: mainslider/ethic.jpg
      - background:
          content: 마음, 나츠메 소세키 
          image:
            filename: mainslider/kokoro.jpg 

  - block: collection
    content:
      id: section-1
      title: Project
      subtitle:
      text:
      count: 3
      offset: 0
      order: desc
      filters:
        folders:
          - linux
          - java
          - remote
    design:
      view: community/custom_card
      columns: '3'

---