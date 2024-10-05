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