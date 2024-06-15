

https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbXlUcDNSTFZZMUVRbTltbGIyUW82T3BHa1dBZ3xBQ3Jtc0trMm85STY0bXBZUl9iUmtFVi03UDUzRWV2Q2Vnd2EtNW9LWWZBaGRuSUxRQTRyUk42RmlTVW9vRm9OQURxOGc3SlEza3pwUjZaNDhDZXJfTnNkd0ZrdVJKQ3Z4VUE0d2UzY1lwX0tIZDhyaW9KRkRGbw&q=https%3A%2F%2Fcolab.research.google.com%2F&v=zlh1CESVpv4


➡️使用的两行命令：
!pip install -U chattts-fork



!chattts ' 你好，我是BKT，今天听了个笑话, 我将给你听下。'  --seed 6000




https://chattts.com/



# ChatTTS
[**English**](./README.md) | [**中文简体**](./README_CN.md)

ChatTTS is a text-to-speech model designed specifically for dialogue scenario such as LLM assistant. It supports both English and Chinese languages. Our model is trained with 100,000+ hours composed of chinese and english. The open-source version on **[HuggingFace](https://huggingface.co/2Noise/ChatTTS)** is a 40,000 hours pre trained model without SFT.

For formal inquiries about model and roadmap, please contact us at **open-source@2noise.com**. You could join our QQ group: 808364215 for discussion. Adding github issues is always welcomed.

---
## Highlights
1. **Conversational TTS**: ChatTTS is optimized for dialogue-based tasks, enabling natural and expressive speech synthesis. It supports multiple speakers, facilitating interactive conversations.
2. **Fine-grained Control**: The model could predict and control fine-grained prosodic features, including laughter, pauses, and interjections. 
3. **Better Prosody**: ChatTTS surpasses most of open-source TTS models in terms of prosody. We provide pretrained models to support further research and development.

For the detailed description of the model, you can refer to **[video on Bilibili](https://www.bilibili.com/video/BV1zn4y1o7iV)**

---

## Disclaimer

This repo is for academic purposes only. It is intended for educational and research use, and should not be used for any commercial or legal purposes. The authors do not guarantee the accuracy, completeness, or reliability of the information. The information and data used in this repo, are for academic and research purposes only. The data obtained from publicly available sources, and the authors do not claim any ownership or copyright over the data.

ChatTTS is a powerful text-to-speech system. However, it is very important to utilize this technology responsibly and ethically. To limit the use of ChatTTS, we added a small amount of high-frequency noise during the training of the 40,000-hour model, and compressed the audio quality as much as possible using MP3 format, to prevent malicious actors from potentially using it for criminal purposes. At the same time, we have internally trained a detection model and plan to open-source it in the future.


---
## Usage

<h4>Basic usage</h4>

```python
(诗歌)
我想去国外旅游，追逐那未知的梦幻，
带着行李和期待，走向那遥远的地方。

(副歌)
我想去国外旅游，感受那异国的风情，
在陌生的城市中，寻找属于自己的瞬间。

(诗歌)
我想去国外旅游，看看那辽阔的海洋，
在沙滩上留下足迹，记录这美好的时光。

(副歌)
我想去国外旅游，体验那不同的文化，
在每一个角落里，都有新的故事等待着我。

(桥段)
我想去国外旅游，让心灵得到释放，
在这次旅行中，我会找到自己的方向。

(副歌)
我想去国外旅游，这是我心中的愿望，
无论前方的路有多长，我都会勇往直前。

(尾声)
我想去国外旅游，这是我最深的渴望，
带着梦想和希望，我会踏上那未知的旅程。

<details open>
  <summary><h4>Example: self introduction</h4></summary>

```python
inputs_en = """
chat T T S is a text to speech model designed for dialogue applications. 
[uv_break]it supports mixed language input [uv_break]and offers multi speaker 
capabilities with precise control over prosodic elements [laugh]like like 
[uv_break]laughter[laugh], [uv_break]pauses, [uv_break]and intonation. 
[uv_break]it delivers natural and expressive speech,[uv_break]so please
[uv_break] use the project responsibly at your own risk.[uv_break]
""".replace('\n', '') # English is still experimental.

params_refine_text = {
  'prompt': '[oral_2][laugh_0][break_4]'
} 
# audio_array_cn = chat.infer(inputs_cn, params_refine_text=params_refine_text)
audio_array_en = chat.infer(inputs_en, params_refine_text=params_refine_text)
torchaudio.save("output3.wav", torch.from_numpy(audio_array_en[0]), 24000)
```
[male speaker](https://github.com/2noise/ChatTTS/assets/130631963/e0f51251-db7f-4d39-a0e9-3e095bb65de1)

[female speaker](https://github.com/2noise/ChatTTS/assets/130631963/f5dcdd01-1091-47c5-8241-c4f6aaaa8bbd)
</details>

---
## Roadmap
- [x] Open-source the 40k hour base model and spk_stats file
- [ ] Open-source VQ encoder and Lora training code
- [ ] Streaming audio generation without refining the text*
- [ ] Open-source the 40k hour version with multi-emotion control
- [ ] ChatTTS.cpp maybe? (PR or new repo are welcomed.)
 
----
## FAQ

##### How much VRAM do I need? How about infer speed?
For a 30-second audio clip, at least 4GB of GPU memory is required. For the 4090 GPU, it can generate audio corresponding to approximately 7 semantic tokens per second. The Real-Time Factor (RTF) is around 0.3.

##### model stability is not good enough, with issues such as multi speakers or poor audio quality.

This is a problem that typically occurs with autoregressive models(for bark and valle). It's generally difficult to avoid. One can try multiple samples to find a suitable result.

##### Besides laughter, can we control anything else? Can we control other emotions?

In the current released model, the only token-level control units are [laugh], [uv_break], and [lbreak]. In future versions, we may open-source models with additional emotional control capabilities.

---
## Acknowledgements
- [bark](https://github.com/suno-ai/bark), [XTTSv2](https://github.com/coqui-ai/TTS) and [valle](https://arxiv.org/abs/2301.02111) demostrate a remarkable TTS result by a autoregressive-style system.
- [fish-speech](https://github.com/fishaudio/fish-speech) reveals capability of GVQ as audio tokenizer for LLM modeling.
- [vocos](https://github.com/gemelo-ai/vocos) which is used as a pretrained vocoder.

---
## Special Appreciation
- [wlu-audio lab](https://audio.westlake.edu.cn/) for early algorithm experiments.
