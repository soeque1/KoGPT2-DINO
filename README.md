# KoGPT2-DINO
- KoGPT2μ Datasets from Instructions (DINO π¦)λ₯Ό μ΄μ©νμ¬ λ°μ΄ν°λ₯Ό μμ±νλ μ΄νλ¦¬μΌμ΄μμλλ€.
- νμ΅λ λͺ¨νκ³Ό Config λ±μ μν΄ μ±λ₯μ λ¬λΌμ§ μ μμ΅λλ€.

## μμ

Input:
- λ¬Έμ₯: GPT-2λ μ£Όμ΄μ§ νμ€νΈμ λ€μ λ¨μ΄λ₯Ό μ μμΈ‘ν  μ μλλ‘ νμ΅λ μΈμ΄λͺ¨λΈμλλ€.

Output (νμΈνλ μμ΄ μμ±):
- λμΌ μλ―Έ: "GPT-2λ μ£Όμ΄μ§ νμ€νΈμ λ€μ λ¨μ΄λ₯Ό μ νν μμΈ‘ν  μ μλλ‘ νμ΅λ μΈμ΄λͺ¨λΈμλλ€."
- μ μ¬ μλ―Έ: "GPT-2λ νμ€νΈμ λ€μ λ¨μ΄λ₯Ό μμΈ‘νλλ‘ κ³ μλ μΈμ΄λͺ¨λΈμλλ€."
- λ€λ₯Έ μ£Όμ : "GPT-2μ μ²« λ¬Έμ₯μ μ μμΈ‘ν  μ μκ²λ μ°μ΅νμ­μμ€."


References:
- https://github.com/SKT-AI/KoGPT2
- https://github.com/timoschick/dino

--------------------------------
## HOT TO USE

## Prepare

```sh
git clone https://github.com/soeque1/KoGPT2-DINO.git --recursive
cd KoGPT2-DINO
```

## Input

```sh
cat test_input.txt
```

> GPT-2λ μ£Όμ΄μ§ νμ€νΈμ λ€μ λ¨μ΄λ₯Ό μ μμΈ‘ν  μ μλλ‘ νμ΅λ μΈμ΄λͺ¨λΈμλλ€.

## Config

```sh
cat task_specs/para-ko.json
pip install -r requirements.txt
```

```json
{
    "task_name": "para-ko",
    "labels": {
      "2": {
        "instruction": "κ³Όμ  : λμΌν μλ―Έμ λ λ¬Έμ₯μ μμ±νμΈμ.\nλ¬Έμ₯ 1: \"<X1>\"\nλ¬Έμ₯ 2: \"",
        "counter_labels": []
      },
      "1": {
        "instruction": "κ³Όμ  : λΉμ·ν μλ―Έμ λ λ¬Έμ₯μ μμ±νμΈμ.\nλ¬Έμ₯ 1: \"<X1>\"\nλ¬Έμ₯ 2: \"",
        "counter_labels": [
          "2"
        ]
      },
      "0": {
        "instruction": "κ³Όμ  : μμ ν λ€λ₯Έ μ£Όμ μ κ΄νμ¬ λ λ¬Έμ₯μ μμ±νμΈμ.\nλ¬Έμ₯ 1: \"<X1>\"\nλ¬Έμ₯ 2: \"",
        "counter_labels": [
          "2",
          "1"
        ]
      }
    }
  }
```

## Run

- ONLY CPU
```sh
python dino/dino.py \
 --output_dir test_out \
 --model_name skt/kogpt2-base-v2 \
 --task_file task_specs/para-ko.json \
 --input_file test_input.txt \
 --input_file_type plain \
 --max_output_length 128 \
 --top_p 0.9 \
 --top_k 5 \
 --decay_constant 100 \
 --remove_identical_pairs \
 --num_entries_per_input_and_label 1 \
 --no_cuda
```

- GPU
```
python dino/dino.py \
 --output_dir test_out \
 --model_name skt/kogpt2-base-v2 \
 --task_file task_specs/para-ko.json \
 --input_file test_input.txt \
 --input_file_type plain \
 --max_output_length 128 \
 --top_p 0.9 \
 --top_k 5 \
 --decay_constant 100 \
 --remove_identical_pairs \
 --num_entries_per_input_and_label 1
```

- DOCKER (GPU)
```
docker run \
  --gpus 1 \
  -v $PWD:/workspace \
  nvcr.io/nvidia/pytorch:20.12-py3 \
  bash -c "
  pip install -r requirements.txt &&
  python dino/dino.py \
  --output_dir test_out \
  --model_name skt/kogpt2-base-v2 \
  --task_file task_specs/para-ko.json \
  --input_file test_input.txt \
  --input_file_type plain \
  --max_output_length 128 \
  --top_p 0.9 \
  --top_k 5 \
  --decay_constant 100 \
  --remove_identical_pairs \
  --num_entries_per_input_and_label 1"
```

## Results

```sh
cat test_out/para-ko-dataset.jsonl
```

```json
# λμΌ μλ―Έ λ¬Έμ₯
{
  "text_a": "GPT-2λ μ£Όμ΄μ§ νμ€νΈμ λ€μ λ¨μ΄λ₯Ό μ μμΈ‘ν  μ μλλ‘ νμ΅λ μΈμ΄λͺ¨λΈμλλ€.",
  "text_b": "GPT-2λ μ£Όμ΄μ§ νμ€νΈμ λ€μ λ¨μ΄λ₯Ό μ νν μμΈ‘ν  μ μλλ‘ νμ΅λ μΈμ΄λͺ¨λΈμλλ€.",
  "label": "2"
}

# μ μ¬ μλ―Έ λ¬Έμ₯
{
  "text_a": "GPT-2λ μ£Όμ΄μ§ νμ€νΈμ λ€μ λ¨μ΄λ₯Ό μ μμΈ‘ν  μ μλλ‘ νμ΅λ μΈμ΄λͺ¨λΈμλλ€.",
  "text_b": "GPT-2λ νμ€νΈμ λ€μ λ¨μ΄λ₯Ό μμΈ‘νλλ‘ κ³ μλ μΈμ΄λͺ¨λΈμλλ€.",
  "label": "1"
}

# λ€λ₯Έ μ£Όμ  λ¬Έμ₯
{
  "text_a": "GPT-2λ μ£Όμ΄μ§ νμ€νΈμ λ€μ λ¨μ΄λ₯Ό μ μμΈ‘ν  μ μλλ‘ νμ΅λ μΈμ΄λͺ¨λΈμλλ€.",
  "text_b": "GPT-2μ μ²« λ¬Έμ₯μ μ μμΈ‘ν  μ μκ²λ μ°μ΅νμ­μμ€.",
  "label": "0"
}
```