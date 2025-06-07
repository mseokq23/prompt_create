# 지화 프로젝트 프롬프트 전송용 레포

```bash
git clone https://github.com/mseokq23/prompt_create.git
```

위의 명령어로 라즈베리파이에 클론하시면 됩니다.(실행 위치 /home/jion/jihwa/)

그럼 jihwa 폴더 내에 "prompt_create" 폴더가 생성되는데, 이를 사용하기 위해서는 prompts 폴더에 각 json 파일을 옮겨야합니다.

아래의 명령어를 이용해서 옮기세요.

```bash
cp /home/jion/jihwa/prompt_create/[프롬프트 이름] /home/jion/jihwa/prompts/[프롬프트 이름]
```

빠른 복제를 위해 모든 파일은 소문자로 작성해주세요

## 추가) 프롬프트를 생성할 때, 특징

코드를 보면, 프롬프트 조각(프래그먼트)은 JSON 파일에서 리스트의 리스트 형태로 로드되며,
각 리스트(프래그먼트 그룹)마다 1개씩 선택해서 프롬프트를 만듭니다.
즉, 리스트가 몇 개이든 상관없이 동작합니다.

정리:

프롬프트 조각(리스트)의 개수는 프롬프트 파일(JSON)에 따라 자유롭게 늘릴 수 있습니다.
코드에서 리스트 개수에 대한 제한을 두지 않습니다.
리스트가 2개면 2개 조각, 10개면 10개 조각을 조합해서 프롬프트가 만들어집니다.

```json
[
  ["a cat", "a dog"],
  ["in a garden", "on the moon"],
  ["sunny", "rainy"]
]
```

"몇 개"를 사용하는지는 프롬프트 파일(JSON) 안에 포함된 리스트의 개수에 따라 달라집니다. 예를 들어, 프롬프트 파일에 3개의 리스트가 있으면, 3개의 프래그먼트가 조합되어 하나의 프롬프트가 생성됩니다.


이 코드에서 "prompt fragment"를 가져오는 방식은 다음과 같습니다:

프롬프트 파일 로드:

load_prompts(prompt_file: str) -> List[List[str]] 함수에서 JSON 파일(기본값: prompts/flowers.json)을 불러옵니다.
이 프롬프트 파일은 "프롬프트 조각(fragment)"의 중첩 리스트 형태입니다. 즉, 여러 개의 리스트(각각이 한 그룹의 프래그먼트)를 포함하는 리스트입니다.
프롬프트 조합:

generate_prompt(prompts: List[List[str]], custom_prompt: str = "") -> str
custom_prompt가 지정되어 있으면 프래그먼트 대신 그것을 사용합니다.
custom_prompt가 없으면, 각 프래그먼트 그룹(리스트)에서 무작위로 하나씩 선택하여 모두 이어붙여 최종 프롬프트를 만듭니다.
즉, random.choice(fragments) for fragments in prompts를 통해, 각 리스트에서 1개씩 뽑아 ' '.join(...)으로 합칩니다.
