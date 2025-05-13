# 텔레그램 봇을 활용하여 사진 보내기

기본적으로 필요한 라이브러리 설치완료(기억이...)

# 텔레그램 기본 설정
`BotFather`를 활용하여 봇을 만든 뒤 토큰을 저장해둔다.


# git clone

아래 코드를 통해 클론 받는다.
```
git clone https://github.com/python-telegram-bot/python-telegram-bot.git
```


# 봇 설정

클론 받은 파일에 접근하여 `examples/` 로 들어간다.
들어가서 `vim timerbot.py`를 입력하여 문서를 편집한다.
코드를 따라 내려가다 보면 96번줄에 `"TOKEN"` 부분을 아까 받은 봇 토큰을 입력해준다. 오타나면 실행 안됌
```
application = Application.builder().token("TOKEN").build()
```

# 실행

`python timerbot.py` 입력하면 코드가 실행된다.

그리고 텔레그램에 내가 만든 봇에게 대화를 걸고 
`/start`를 입력후
`/set 5`를 입력하면 5초뒤에 답장이 오는 봇이 만들어 진다.
