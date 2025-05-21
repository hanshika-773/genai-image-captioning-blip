## Prototype Development for Image Captioning Using the BLIP Model and Gradio Framework

### AIM:
To design and deploy a prototype application for image captioning by utilizing the BLIP image-captioning model and integrating it with the Gradio UI framework for user interaction and evaluation.

### PROBLEM STATEMENT:

### DESIGN STEPS:
### STEP 1:
Choose a pre-trained image captioning model (BLIP) and load necessary API credentials.

### STEP 2:
Build a function to convert images into a format suitable for the model and get the caption output.

### STEP 3:
Design a basic interface using Gradio to let users upload images and view the generated captions.

### PROGRAM:
```
import os
import io
import IPython.display
from PIL import Image
import base64 
from dotenv import load_dotenv, find_dotenv
_ = load_dotenv(find_dotenv()) # read local .env file
hf_api_key = os.environ['HF_API_KEY']
# Helper functions
import requests, json

#Image-to-text endpoint
def get_completion(inputs, parameters=None, ENDPOINT_URL=os.environ['HF_API_ITT_BASE']):
    headers = {
      "Authorization": f"Bearer {hf_api_key}",
      "Content-Type": "application/json"
    }
    data = { "inputs": inputs }
    if parameters is not None:
        data.update({"parameters": parameters})
    response = requests.request("POST",
                                ENDPOINT_URL,
                                headers=headers,
                                data=json.dumps(data))
    return json.loads(response.content.decode("utf-8"))
import gradio as gr 

def image_to_base64_str(pil_image):
    byte_arr = io.BytesIO()
    pil_image.save(byte_arr, format='PNG')
    byte_arr = byte_arr.getvalue()
    return str(base64.b64encode(byte_arr).decode('utf-8'))

def captioner(image):
    base64_image = image_to_base64_str(image)
    result = get_completion(base64_image)
    return result[0]['generated_text']

gr.close_all()
demo = gr.Interface(
    fn=captioner,
    inputs=[gr.Image(label="Upload image", type="pil")],
    outputs=[gr.Textbox(label="Caption")],
    title="Image Captioning",
    description="Caption any input image",
    allow_flagging="never"
)

demo.launch(share=True, server_port=int(os.environ['PORT1']))
```

### OUTPUT:
![Screenshot 2025-05-21 224549](https://github.com/user-attachments/assets/3d4e9b2b-fc2e-45d6-bf6f-7875e84328e8)

### RESULT:
A prototype application for image captioning was successfully designed and deployed using the BLIP image-captioning model. The application was integrated with the Gradio UI framework, enabling user interaction and effective evaluation of the model’s performance.
