# Selenium으로 이미지 수집하기

## 실습 목표

Selenium으로 브라우저를 조작해 검색 결과를 스크롤하고, 이미지 주소를 얻어 파일로 저장하는 자동화 흐름을 이해한다.

## 전체 흐름

1. `webdriver.Chrome()`으로 브라우저를 연다.
2. 검색어를 입력하고 검색을 실행한다.
3. 페이지를 스크롤해 이미지를 더 불러온다.
4. 이미지 요소의 `src` 속성을 수집한다.
5. `requests.get()`으로 이미지를 내려받아 저장한다.

## 핵심 코드

```python
driver = webdriver.Chrome()
driver.get(url)

images = driver.find_elements(By.CSS_SELECTOR, "img")

for i, image in enumerate(images):
    image_url = image.get_attribute("src")
    image_data = requests.get(image_url).content

    with open(f"{keyword}_{i}.jpg", "wb") as f:
        f.write(image_data)
```

> [!warning]
> 웹 페이지의 CSS 선택자와 로딩 방식은 바뀔 수 있으므로 수업 코드가 항상 같은 결과를 보장하지는 않는다. `src`가 비어 있는 요소, 요청 실패, 파일 형식 차이도 예외 처리할 필요가 있다.

## 이 실습에서 사용한 개념

- [[while과 for는 언제 사용하는가]]
- [[with open은 왜 사용하는가]]
- [[API 요청과 응답은 어떻게 동작하는가]]
- [[예외 처리는 왜 필요한가]]

## 출처

- `day0724_2.ipynb`

