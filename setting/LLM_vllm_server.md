### Setting

#### 현재 상황
현재, 사용하고 있는 환경은 아래와 같습니다.
- OS: Window
- GPU: RTX 3090 (VRAM: 24GB)

따라서 VLLM Server를 띄우기 위하여 아래와 같은 추가 적인 작업을 진행하였습니다.
1. wsl 설치
2. 해당 ubuntu에서 LLM VLLM을 활용하여 올리기

appendix: model은 GPU에 올리기 위하여 qwen3-4b로 진행하였습니다.

#### VLLM을 활용하여 LLM 띄우기

source ~/LLM/bin/activate

python3 -m vllm.entrypoints.openai.api_server \
  --model Qwen/Qwen3-4B \
  --tokenizer Qwen/Qwen3-4B \
  --dtype auto

#### API Call 확인 하기

1. Prompt 만들기
cat > prompt.json <<EOF
{
  "model": "Qwen/Qwen3-4B",
  "messages": [
    {"role": "user", "content": "서울에 대해서 알려주세요"}
  ],
  "max_tokens": 1024,
  "temperature": 0
}
EOF

2. API 호출 하기
curl -X POST http://localhost:8000/v1/chat/completions \
  -H "Content-Type: application/json" \
  -d @prompt.json