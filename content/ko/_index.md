---
# Leave the homepage title empty to use the site title
title: "전북대 이은정"
description: "전북대 이은정"
keywords: "전북대, 이은정, 전북대 이은정"
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
      title: '📚 어서오세요, 제 블로그에!'
      subtitle: ''
      text: |-
        안녕하세요! 이 블로그는 저의 프로젝트 기록, 대외활동 기록, 독서 기록, 취미 기록 등이 담겨 있는 공간입니다. 자주 들러주세요! 감사합니다 😃
    design:
      columns: '1'
  - block: collection
    id: works
    content:
      title: 프로젝트
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
      title: 대외활동 talk
      filters:
        folders:
          - event
    design:
      view: article-grid
      columns: 3

  - block: collection
    id: news
    content:
      title: 취미 소식
      filters:
        folders:
          - post
    design:
      view: article-grid
      columns: 3


  - block: slider
    content:
      slides:
      - title: <span style="font-size:90%">Welcome!</span>
        content: <span style="font-size:90%">Eunjung's blog<span style="font-size:90%">
        align: center
        background:
          image:
            filename: ian-talmacs-vieCCGMtrx0-unsplash.jpg
            filters:
              brightness: 0.4
          position: center
          color: '#000'

      - title: <span style="font-size:90%">DATA</span>
        content: <span style="font-size:90%">I am interested in data</span>
        align: center
        background:
          image:
            filename: joshua-sortino-LqKhnDzSF-8-unsplash.jpg
            filters:
              brightness: 0.4
          position: center
          color: '#000'

      - title: <span style="font-size:90%">My Favorite</span>
        content: <span style="font-size:90%">Check out my hobbies on the blog!</span>
        align: center
        background:
          image:
            filename: leo-rivas-wtxcaDIdOCM-unsplash.jpg
            filters:
              brightness: 0.4
          position: center
          color: '#000'


  - block: community/slider
    content:
      title: 독서
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
      title: 프로젝트 정리
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