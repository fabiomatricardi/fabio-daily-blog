# FABIO DAILY BLOG
### Daily notes brainstorming and ideas

<img src='https://github.com/fabiomatricardi/fabio-daily-blog/raw/main/brainstorming.jpg' width=900>

---

<a id="toc"></a> 
## Table of Contents
- [Can you build a blog with Gradio?](#gradioblog)
- [Creating Markdown Internal Links](#createmarkdownlinks) 
- [Cool gradio examples](#coolgradio)
- [LSP server for Sublime Text 3 to suggest python libraries arguments](#lsp)
- [Gradio multi-pages app](#gradiomultipages)
- [Stable Diffusion CPP and Flux2](#sdcpp)


---

<a id="gradioblog"></a> 
## Can you build a blog with Gradio?
> Gradio is not a dedicated platform for hosting a full-featured, traditional blog with content management, but it can be used to build a custom, interactive web application in Python that functions as a simple blogging interface or content generator. Gradio excels at quickly creating UIs for Python functions, such as AI-powered text generation. 

#### Example: A Post Editor and Previewer
This code creates a basic interface where a user can write a post title and content in Markdown, and see a preview. 

```python
import gradio as gr
import time

def publish_post(title, content):
    """Simulates publishing a blog post to a simple data store (e.g., a file or database)."""
    if not title or not content:
        return "Error: Title and content cannot be empty."
    
    timestamp = time.strftime("%Y-%m-%d %H:%M:%S")
    post_data = f"## {title}\n*Published: {timestamp}*\n\n---\n{content}\n\n"
    
    # In a real app, you would save this to a database or file
    with open("blog_posts.md", "a") as f:
        f.write(post_data)
        
    return f"Post '{title}' published successfully at {timestamp}!"

with gr.Blocks(title="Gradio Blog Editor") as demo:
    gr.Markdown("# Create a New Blog Post")

    with gr.Row():
        title_input = gr.Textbox(label="Post Title", placeholder="Enter the title...")
    
    with gr.Row():
        content_input = gr.Textbox(label="Content (Markdown supported)", lines=10, placeholder="Write your post here...")
    
    publish_button = gr.Button("Publish Post")
    output_message = gr.Textbox(label="Status")

    # The click event connects the button to the function
    publish_button.click(
        fn=publish_post,
        inputs=[title_input, content_input],
        outputs=output_message
    )
    
    gr.Markdown("---")
    gr.Markdown("### Preview (View the `blog_posts.md` file after publishing)")

demo.launch()
```


[Bak to Table of Contents](#toc)

---


<a id="createmarkdownlinks"></a> 
## Creating Markdown Internal Links
refer to [this link](https://blog.markdowntools.com/posts/markdown-internal-links)

Markdown internal links are a powerful tool to help users navigate your document. These internal links can direct your readers to different sections inside the current page or to a specific part of another Markdown document within the same repository.

#### What are Markdown Internal Links?
Markdown internal links, also known as anchor links, offer a way to link to specific areas within a document. These links come incredibly handy for long documents, providing a seamless navigation experience.

Before diving into how to create internal links, let’s ensure that we understand Markdown’s primary syntax for creating links.
```markdown
[Link Text](URL)
```
This Markdown code will render as a clickable hyperlink with “Link Text” being the clickable text, and “URL” being the webpage the user will be directed to when clicking on the link.

#### Creating Markdown Internal Links
To create an internal link in Markdown:

First, you need to create an ID for the section you wish to link. This is done by adding an anchor tag before the header. The ID can be anything you want, but it’s best to keep it related to the section’s title.
```markdown
<a id="section_id"></a>
### Section Title
```
We’re using a simple HTML anchor tag (<a>) with an id attribute (?section_id?). The anchor tag is self-closed, meaning it doesn’t need a closing </a> tag.

Now that you’ve assigned an ID to the section, you can create an internal link pointing to it. Use the normal syntax for creating a link in Markdown, but instead of a URL, use the ID prefixed with a #.
```markdown
[Section Title](#section_id)
```
When you click on “Section Title,” you will be led to the section with the “section_id.” Here’s an example:
```markdown
<a id="installation"></a>
## Installation

To install the package do...

[Go to Installation](#installation)
```
Clicking “Go to Installation” will take the user to the “Installation” section.

[Bak to Table of Contents](#toc)

---

<a id="coolgradio"></a> 
## Cool gradio examples
From the official repository a collection of demos, to learn.

[https://github.com/gradio-app/gradio/tree/main/demo](https://github.com/gradio-app/gradio/tree/main/demo)

<img src='https://github.com/fabiomatricardi/fabio-daily-blog/raw/main/gradioExamples.jpg' width=800>

[Bak to Table of Contents](#toc)

---

<img src='https://wxguy.in/assets/img/sublime_text/sublime_text_4.webp' width=800>

<a id="lsp"></a> 
## LSP server for Sublime Text 3 to suggest python libraries arguments
To enable code hints (autocompletion and documentation) for imported Python libraries in Sublime Text 3, you need to install and configure a third-party package like LSP with a Python language server (such as pyls or pyright) or the older, but still functional, Anaconda plugin. Using the Language Server Protocol (LSP) client is the modern and recommended approach. 

#### Option 1: Using LSP (Recommended)
The Language Server Protocol (LSP) provides a robust way to get IDE-like features in Sublime Text. <br>
Install Package Control: If you don't have it, follow the instructions on the Package Control website to install it.<br>
Install the LSP package:
```
Open the Command Palette with Ctrl+Shift+P (Windows/Linux) or Cmd+Shift+P (Mac).
Type "Package Control: Install Package" and select it.
Search for `LSP` and install the package.
```
Install a Python Language Server: <br>
You need a language server installed on your system (not in Sublime Text). Popular choices include `pyls` (Python Language Server) or `pyright`.
```
Install one using pip: `pip install python-language-server or pip install pyright`.
Install an LSP-Python helper package in Sublime Text:
Open the Command Palette again.
Select "Package Control: Install Package".
Search for and install a relevant helper package, such as "LSP-pyright" or just "LSP-python".
```

Enable the server in Sublime Text:<br>
Once installed, you may need to enable the server globally via the Command Palette: "LSP: Enable Language Server Globally".<br>
You might also need to configure the settings to point to the correct Python interpreter path, especially if using a virtual environment. <br>
This can be done in Preferences > Package Settings > LSP > Settings. 


Refer to 
- [https://wxguy.in/posts/sublime-text-as-python-ide/](https://wxguy.in/posts/sublime-text-as-python-ide/)
- [https://lsp.sublimetext.io/](https://lsp.sublimetext.io/)


[Bak to Table of Contents](#toc)

---

<a id="gradiomultipages"></a> 
## Gradio Multi-pages app
refer to [https://www.gradio.app/guides/multipage-apps](https://www.gradio.app/guides/multipage-apps)

Your Gradio app can support multiple pages with the Blocks.route() method.

All of these pages will share the same backend, including the same queue.

> **Note: multipage apps do not support interactions between pages, e.g. an event listener on one page cannot output to a component on another page. Use gr.Tabs() for this type of functionality instead of pages**.

#### Separate Files

For maintainability, you may want to write the code for different pages in different files. Because any Gradio Blocks can be imported and rendered inside another Blocks using the .render() method, you can do this as follows.

Create one main file, say app.py and create separate Python files for each page:
```
- app.py
- main_page.py
- second_page.py
```
The Python file corresponding to each page should consist of a regular Gradio Blocks, Interface, or ChatInterface application, e.g.

**main_page.py**
```python
import gradio as gr

with gr.Blocks() as demo:
    gr.Image()

if __name__ == "__main__":
    demo.launch()
```

**second_page.py**
```python
import gradio as gr

with gr.Blocks() as demo:
    t = gr.Textbox()
    demo.load(lambda : "Loaded", None, t)

if __name__ == "__main__":
    demo.launch()
```

#### In your main app.py file, simply import the Gradio demos from the page files and .render() them:

**app.py**
```python
import gradio as gr

import main_page, second_page

with gr.Blocks() as demo:
    main_page.demo.render()
with demo.route("Second Page"):
    second_page.demo.render()

if __name__ == "__main__":
    demo.launch()
```

This allows you to run each page as an independent Gradio app for testing, while also creating a single file app.py that serves as the entrypoint for the complete multipage app.

#### ⚠️❗ Remember that all the single apps must be called `demo` and only the main app.py will call for the `.launch()` arguments

[Bak to Table of Contents](#toc)

---

<a id="sdcpp"></a> 
## Stable Diffusion CPP and Flux2
from [https://github.com/leejet/stable-diffusion.cpp/blob/master/docs/flux2.md](https://github.com/leejet/stable-diffusion.cpp/blob/master/docs/flux2.md)
### Flux.2-dev

#### Download weights

- Download FLUX.2-dev
    - gguf: [https://huggingface.co/city96/FLUX.2-dev-gguf/tree/main](https://huggingface.co/city96/FLUX.2-dev-gguf/tree/main)
- Download vae
    - safetensors: [https://huggingface.co/black-forest-labs/FLUX.2-dev/tree/main](https://huggingface.co/black-forest-labs/FLUX.2-dev/tree/main)
- Download Mistral-Small-3.2-24B-Instruct-2506-GGUF
    - gguf: [https://huggingface.co/unsloth/Mistral-Small-3.2-24B-Instruct-2506-GGUF/tree/main](https://huggingface.co/unsloth/Mistral-Small-3.2-24B-Instruct-2506-GGUF/tree/main)

#### Examples

```
.\bin\Release\sd-cli.exe --diffusion-model  ..\..\ComfyUI\models\diffusion_models\flux2-dev-Q4_K_S.gguf --vae ..\..\ComfyUI\models\vae\flux2_ae.safetensors  --llm ..\..\ComfyUI\models\text_encoders\Mistral-Small-3.2-24B-Instruct-2506-Q4_K_M.gguf -r .\kontext_input.png -p "change 'flux.cpp' to 'flux2-dev.cpp'" --cfg-scale 1.0 --sampling-method euler -v --diffusion-fa --offload-to-cpu
```

<img alt="flux2 example" src="https://github.com/leejet/stable-diffusion.cpp/raw/master/assets/flux2/example.png" />

### Flux.2 klein 4B / Flux.2 klein base 4B

#### Download weights

- Download FLUX.2-klein-4B
    - safetensors: [https://huggingface.co/black-forest-labs/FLUX.2-klein-4B](https://huggingface.co/black-forest-labs/FLUX.2-klein-4B)
    - gguf: [https://huggingface.co/leejet/FLUX.2-klein-4B-GGUF/tree/main](https://huggingface.co/leejet/FLUX.2-klein-4B-GGUF/tree/main)
- Download FLUX.2-klein-base-4B
    - safetensors: [https://huggingface.co/black-forest-labs/FLUX.2-klein-base-4B](https://huggingface.co/black-forest-labs/FLUX.2-klein-base-4B)
    - gguf: [https://huggingface.co/leejet/FLUX.2-klein-base-4B-GGUF/tree/main](https://huggingface.co/leejet/FLUX.2-klein-base-4B-GGUF/tree/main)
- Download vae
    - safetensors: [https://huggingface.co/black-forest-labs/FLUX.2-dev/tree/main](https://huggingface.co/black-forest-labs/FLUX.2-dev/tree/main)
- Download Qwen3 4b
    - safetensors: [https://huggingface.co/Comfy-Org/flux2-klein-4B/tree/main/split_files/text_encoders](https://huggingface.co/Comfy-Org/flux2-klein-4B/tree/main/split_files/text_encoders)
    - gguf: [https://huggingface.co/unsloth/Qwen3-4B-GGUF/tree/main](https://huggingface.co/unsloth/Qwen3-4B-GGUF/tree/main)

### Examples

```
.\bin\Release\sd-cli.exe --diffusion-model  ..\..\ComfyUI\models\diffusion_models\flux-2-klein-4b.safetensors --vae ..\..\ComfyUI\models\vae\flux2_ae.safetensors  --llm ..\..\ComfyUI\models\text_encoders\qwen_3_4b.safetensors -p "a lovely cat" --cfg-scale 1.0 --steps 4 -v --offload-to-cpu --diffusion-fa
```

<img alt="flux2-klein-4b" src="https://github.com/leejet/stable-diffusion.cpp/raw/master/assets/flux2/flux2-klein-4b.png" />

```
.\bin\Release\sd-cli.exe --diffusion-model  ..\..\ComfyUI\models\diffusion_models\flux-2-klein-4b.safetensors --vae ..\..\ComfyUI\models\vae\flux2_ae.safetensors  --llm ..\..\ComfyUI\models\text_encoders\qwen_3_4b.safetensors -r .\kontext_input.png -p "change 'flux.cpp' to 'klein.cpp'" --cfg-scale 1.0 --sampling-method euler -v --diffusion-fa --offload-to-cpu --steps 4
```

<img alt="flux2-klein-4b-edit" src="https://github.com/leejet/stable-diffusion.cpp/raw/master/assets/flux2/flux2-klein-4b-edit.png" />

```
.\bin\Release\sd-cli.exe --diffusion-model  ..\..\ComfyUI\models\diffusion_models\flux-2-klein-base-4b.safetensors --vae ..\..\ComfyUI\models\vae\flux2_ae.safetensors  --llm ..\..\ComfyUI\models\text_encoders\qwen_3_4b.safetensors -p "a lovely cat" --cfg-scale 4.0 --steps 20 -v --offload-to-cpu --diffusion-fa
```

<img alt="flux2-klein-base-4b" src="https://github.com/leejet/stable-diffusion.cpp/raw/master/assets/flux2/flux2-klein-base-4b.png" />

### Flux.2 klein 9B / Flux.2 klein base 9B

#### Download weights

- Download FLUX.2-klein-9B
    - safetensors: [https://huggingface.co/black-forest-labs/FLUX.2-klein-9B](https://huggingface.co/black-forest-labs/FLUX.2-klein-9B)
    - gguf: [https://huggingface.co/leejet/FLUX.2-klein-9B-GGUF/tree/main](https://huggingface.co/leejet/FLUX.2-klein-9B-GGUF/tree/main)
- Download FLUX.2-klein-base-9B
    - safetensors: [https://huggingface.co/black-forest-labs/FLUX.2-klein-base-9B](https://huggingface.co/black-forest-labs/FLUX.2-klein-base-9B)
    - gguf: [https://huggingface.co/leejet/FLUX.2-klein-base-9B-GGUF/tree/main](https://huggingface.co/leejet/FLUX.2-klein-base-9B-GGUF/tree/main)
- Download vae
    - safetensors: [https://huggingface.co/black-forest-labs/FLUX.2-dev/tree/main](https://huggingface.co/black-forest-labs/FLUX.2-dev/tree/main)
- Download Qwen3 8B
    - safetensors: [https://huggingface.co/Comfy-Org/flux2-klein-9B/tree/main/split_files/text_encoders](https://huggingface.co/Comfy-Org/flux2-klein-9B/tree/main/split_files/text_encoders)
    - gguf: [https://huggingface.co/unsloth/Qwen3-8B-GGUF/tree/main](https://huggingface.co/unsloth/Qwen3-8B-GGUF/tree/main)

### Examples

```
.\bin\Release\sd-cli.exe --diffusion-model  ..\..\ComfyUI\models\diffusion_models\flux-2-klein-9b.safetensors --vae ..\..\ComfyUI\models\vae\flux2_ae.safetensors  --llm ..\..\ComfyUI\models\text_encoders\qwen_3_8b.safetensors -p "a lovely cat" --cfg-scale 1.0 --steps 4 -v --offload-to-cpu --diffusion-fa
```

<img alt="flux2-klein-9b" src="https://github.com/leejet/stable-diffusion.cpp/raw/master/assets/flux2/flux2-klein-9b.png" />

```
.\bin\Release\sd-cli.exe --diffusion-model  ..\..\ComfyUI\models\diffusion_models\flux-2-klein-9b.safetensors --vae ..\..\ComfyUI\models\vae\flux2_ae.safetensors  --llm ..\..\ComfyUI\models\text_encoders\qwen_3_8b.safetensors -r .\kontext_input.png -p "change 'flux.cpp' to 'klein.cpp'" --cfg-scale 1.0 --sampling-method euler -v --diffusion-fa --offload-to-cpu --steps 4
```

<img alt="flux2-klein-9b-edit" src="https://github.com/leejet/stable-diffusion.cpp/raw/master/assets/flux2/flux2-klein-9b-edit.png" />

```
.\bin\Release\sd-cli.exe --diffusion-model  ..\..\ComfyUI\models\diffusion_models\flux-2-klein-base-9b.safetensors --vae ..\..\ComfyUI\models\vae\flux2_ae.safetensors  --llm ..\..\ComfyUI\models\text_encoders\qwen_3_8b.safetensors -p "a lovely cat" --cfg-scale 4.0 --steps 20 -v --offload-to-cpu --diffusion-fa
```

<img alt="flux2-klein-base-9b" src="https://github.com/leejet/stable-diffusion.cpp/raw/master/assets/flux2/flux2-klein-base-9b.png" />


---








