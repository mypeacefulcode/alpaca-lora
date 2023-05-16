# Alpaca-lora fine-tuning

- https://github.com/tloen/alpaca-lora 를 이용하여 한국어 코퍼스로 학습을 진행했습니다.
- 한국어 코퍼스는 https://github.com/Beomi/KoAlpaca 의 ko_alpaca_data.json 데이터 5만여건을 사용했습니다.
- 메타의 Llama와 스탠포드 대학의 Alpaca는 거대 기업의 전유물화 되어가던 LLM을 개인과 소규모 서비스기업의 손에 올려준 획기적인 모델입니다. 그리고 여기 Alpaca-lora는 low-rank adaptation(LoRA)를 이용하여 스탠포드 대학의 Alpaca를 재현한 레포입니다.

학습은 Colab A100 단일 GPU에서 진행했고 1Epoch를 학습시키기 위해 3시간이 필요했습니다. 백본 모델로 사용된 'decapoda-research/llama-7b-hf'는 한국어에 대한 학습량이 부족하여 더 높은 활용도를 위해선 보다 많은 한국어 데이터에 대한 학습이 필요해 보입니다.

Training
```bash
pip install -r requirements.txt -q
```
학습된 모델의 bin파일의 사이즈가 443byte인 버그가 있어서 아래의 버전으로 설치해야합니다. tloen#293

```bash
pip uninstall peft -y
pip install git+https://github.com/huggingface/peft.git@e536616888d51b453ed354a6f1e243fecb02ea08
```
학습 시간 문제로 1 Epoch를 학습했고, 로그는 WandB에서 확인했습니다.

```bash
python finetune.py \
    --base_model 'decapoda-research/llama-7b-hf' \
    --data_path './ko_alpaca_data.json' \
    --output_dir './lora-alpaca' \
    --num_epochs 1 \
    --wandb_project 'alpaka-lora' \
    --cache_dir '../.cache/huggingface'
```

![스크린샷 2023-05-16 오전 11 21 10](https://github.com/mypeacefulcode/alpaca-lora/assets/16236194/e23e8174-594c-4744-9c5f-04446c36aea8)

실행 예제 
![스크린샷 2023-05-16 오전 11 17 35](https://github.com/mypeacefulcode/alpaca-lora/assets/16236194/5a8e56b2-2313-4a2b-9c7e-d67cc20a104a)

학습 데이터가 적고 1 Epoch 만 학습을 진행해서 성능에 대해 언급하기는 어렵지만, 좀더 한국어가 잘 학습된 백본 모델이 있으면 좋겠다는 생각이 들었습니다.
