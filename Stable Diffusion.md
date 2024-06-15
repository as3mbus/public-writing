---
type: Map Of Content
State: On-Going (Draft)
---

[[Generative Pre-Trained Transformer (GPT)|GPT]] Implementation to generate images

# Preface
## Well Known Model

1. Stable Diffusion
2. Dall-E
3. Midjourney
4. Novel AI
5. Pony


## Usage Learning Source : 
1. creating AI Art channel - https://www.youtube.com/@Not4Talent_AI
2. Complex Image Generation - https://www.youtube.com/watch?v=kfoA0xWv-0Y

## Repository / database : 
1. github
2. https://civitai.com/
3. huggingface
4. https://tensor.art/

## UI Tools
- web ui - https://civitai.com/articles/5066/how-to-choose-a-webui-for-stable-diffusion
- Inpainting and outpainting filling space with prompt and text - https://www.youtube.com/watch?v=ZPEwPEBCY1c
- LoRA Training web UI - https://github.com/bmaltais/kohya_ss

## Tool Details

- Example Web UI
	- Invoke AI - inpainting out painting
	- automatic 1111 - foundation extensible and versatile
	- Lama Cleaner - inpainting - outpainting
	- kohya gui - Lora & Embedding Train
	- comfyui

Interface that integrate Model and maybe multiple model to create AI Pipeline which include generation and maybe training

Example : 
- automatic 111 (Image Generation + Depth Estimation)  https://www.youtube.com/watch?v=TmOtHWNnPZM
- automatic 1111 (pix2pix instruct) (instruction prompt + img 2 img) https://www.youtube.com/watch?v=ln1RMEPKx3Q
- gif2gif https://www.youtube.com/watch?v=F0oaHFVUSeY
- controlnet https://www.youtube.com/watch?v=OxFcIv8Gq8o
	- Open Pose
		- Rotate Character https://www.youtube.com/watch?v=xMmCZ1EqMVA
		- Blender posing https://www.youtube.com/watch?v=ptEZQrKgHAg
		- hand https://www.youtube.com/watch?v=EwWkLMhR23I
	- Latent Couple https://www.youtube.com/watch?v=uR89wZMXiJ8
	- Transfer Style https://www.youtube.com/watch?v=PbDdtPTYm_4
	- 

		![[Pasted image 20240426094842.png]]
		Stable Diffusion Web UI can also train LoRA and  Dreambooth

# Prompting
- copy generation data from output
- struggle with natural language
- use enhancers (e.g : masterpiece, cinematic shot, vignette, centered, hdr, intricate details, hdr, hyper detailed)
- png info can be used to extract generation data if you forgot (stored in meta data)
- weight are higher based on order. first are higher than last
- Prompt structure : image type (illustration, raw photo, selfies ) "of" - Main Subject - Subject action - Object / environment - art style
- utilize same seed to regenerate similar type with adjusted prompt
- iterate order : prompt - seed lock - prompt - cfg scale - sampling steps
- use script (XYZ plot for) 
	- cfg - current, 5 ,7, 10
	- sampling steps - 25, 10, 30, 45
	- sampler - euler a, dpm++ 2m karras, unipc
- prompt formatting detail
	- weighting - highlight text, `ctrl + up / down arrow` adjust weight `(selection:<weight>)`
	- switch (flip flop) - `[textA | textB]` switch every step (can put more than 2)
	- switch at  - `[textA : Text B : change at]` switch at specific step can also use float 0~1 to change at percentage
	- add at  - `[textA : X]` add keyword at sample step X
	- remove - `[textA :: X]` remove keyword at sample step X

## Variable Auto Encoder (VAE)

https://civitai.com/articles/115/sd-basics-vae-what-it-is-comparison-how-to-install
work 

# Iterating Output to Create Originality and Quality

- can start from your prompted concept image (read more in [[#Prompting]]) / sketch 
- add Trained Model (LoRA, Dreambooth, Textual Inversion Embedding, )
- img2img (inpaint, outpaint)
- control net
## Training Model

### Textual Inversion 1.0 
- Hugging Face Group : SD Concept
- Training Colab doc: [https://colab.research.google.com/git...](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbDJ2dG1UQkNiVHJaZzhUMGZKVGE4dzVmcVlwUXxBQ3Jtc0trWjlhOTN1aUZTak5YekRHRkY4UEx1dDJsTEUzYkxJc1gzSmJTUDFiY2Y4SWZ4S0pScFBFZy1fODJsTzRJd2RiRmdTRF9UbVRGY0ctXzZIU1VfQWh3NDJqdWk0ZjctQlhQWlNOYnhuX09yX1M3N2VNQQ&q=https%3A%2F%2Fcolab.research.google.com%2Fgithub%2Fhuggingface%2Fnotebooks%2Fblob%2Fmain%2Fdiffusers%2Fsd_textual_inversion_training.ipynb&v=7OnZ_I5dYgw) 
- Inference Colab doc: [https://colab.research.google.com/git...](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbnZ3d0gxb3pkSWdCTjBoOVdoTjQ1YXlVX0tTZ3xBQ3Jtc0tsZ3dVLTdjckMwMGs5NVY3N2lUcm5VTkhtUmF6Q1FCQXlrNlBsdXA0RTFmRzhfQ1R3RE1GQTk3V0lsMHBQcHVGQUZHa216aExsMVJnd3BCbkZ1ZmlzX0x4Z1gtQ1J5N0ZXdDQxbDQxTkFERjhDWDBKaw&q=https%3A%2F%2Fcolab.research.google.com%2Fgithub%2Fhuggingface%2Fnotebooks%2Fblob%2Fmain%2Fdiffusers%2Fstable_conceptualizer_inference.ipynb&v=7OnZ_I5dYgw) 
- Hugging face: [https://huggingface.co/settings/tokens](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqblZGNDBYUF9fZjRzUVd3aW9fTmswX0ktVlZsQXxBQ3Jtc0ttZmtYTXlXVzlqcFFhR3ZYWXZRRTl2emh0XzZraFFLYUxPZldncUEybDBFS0YzR05Gb1N1dmNBZVBjTzlyVVFfblJONmdrc3QyTXg2X3N1ZnI5VWZWVlc3MXVSV05nMGdkSkNkdV8tLUFldXVRNzYxbw&q=https%3A%2F%2Fhuggingface.co%2Fsettings%2Ftokens&v=7OnZ_I5dYgw)
- https://www.youtube.com/watch?v=7OnZ_I5dYgw&t=76s
### Hypernetwork
	- TLDR : Not Recommended. takes lot of operating and iteration
		- Input: Img (will be marked / tagged)
		- Output: Pt File
### Textual Inversion 2.0 (Dream Booth)
Hugging Face Group : SD Dreambooth Library
Training Tutorial : https://www.youtube.com/watch?v=tgRiZzwSdXg
Dreambooth https://github.com/JoePenna/Dreambooth-Stable-Diffusion
TLDR : In Short : inject more data into existing model with specific keyword specified for it
	Input : Image + existing model ckpt
	Output : updated ckpt
	- require 24 gb vram
	- can rent from https://vast.ai/ or https://www.runpod.io/
Bonus : can also use existing training technique / record called diffuser (?)
https://gist.github.com/jachiam/8a5c0b607e38fcc585168b90c686eb05

Faster Google Collab : https://colab.research.google.com/github/TheLastBen/fast-stable-diffusion/blob/main/fast-DreamBooth.ipynb

Local : https://www.youtube.com/watch?v=HahKXY7AQ8c
	Learning Steps: 1200
	Learning Rate : 1 e-6
	use 8bit adam : true (for lower gpu)
	mixed precision : fp 16
### Textual Inversion Embedding 
Input: training image (optional with caption), Configurations, ckpt
Ouput : .pt file (10 kb)
Learning Rate : Brush Size to cut Model to our preferred output
Large leaves the following
![[Pasted image 20240426104942.png]]
Smaller : result the following (slower)
![[Pasted image 20240426105033.png]]
TLDR : Training into model without creating new ckpt (smaller size for easier injection)
https://www.youtube.com/watch?v=2ityl_dNRNw
### LoRA Training Embedding
- Input : Images, regluarisation image, base model ckpt, configuration
- output : LoRA .safetensor
- concept : https://huggingface.co/blog/lora
- general - https://www.youtube.com/watch?v=70H03cv57-o
- detailed training tutorial - https://www.youtube.com/watch?v=xXNr9mrdV7s
### LoRA Extraction
- input : trained model .ckpt, base model .ckpt , and feature information
- Output : LoRA .safetensor
- extract feature from model and use it easier with LoRA Embedding
- https://www.youtube.com/watch?v=CnoXyIcXQV4

## Img2Img


## Control Net

# Bookmark


Novel AI
> [!cite] [GitHub - towa0131/novelai-colab: NovelAI on Google Colaboratory](https://github.com/towa0131/novelai-colab/tree/master)
> <img src="https://opengraph.githubassets.com/18bbf864680fc35dbba7a3df1efb80cf5036ab48ba1e744e5bdef08ac4337b73/towa0131/novelai-colab" alt="Image" style="max-width: 100%; max-height: 100px; float: right; clear: right; margin-left: 1rem;margin-bottom: 2px;margin-top: 2px;"/> NovelAI on Google Colaboratory. Contribute to towa0131/novelai-colab development by creating an account on GitHub.

run novel ai guide https://aituts.com/run-novelai-image-generator-locally/
