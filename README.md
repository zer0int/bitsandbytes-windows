## Bits & Bytes for Windows - bitsandbytes 0.44.1.dev0 for Python 3.10, CUDA 12.6

⚠️ Note: You probably shouldn't just download and use this. It is executable code. It could contain any malware / virus / trojan you can imagine. If I told you the file is safe, could you trust it? No. I am the only person in the world who knows whether this is safe, or contains sneaky malware. Stuff on GitHub is wild mix of benevolent, experienced people offering kind help with compiled versions of just about anything that doesn't natively work on Windows. But as there's a current AI frenzy and everybody wants to fine-tune, make LoRAs and whatnot, it's also 'worth it' for malign actors to sneak in malicious code in their 'kind offer'. So, the stuff you download may work and fix your problem -- while also dropping a Trojan Horse on your system. 👹🦠👺

- Build yourself a chatbot. Don't become part of a botnet. 🤗
- In the *very least*, you should upload any wheels or dll files to online malware scanners, such as [www.virustotal.com/gui/home/upload](https://www.virustotal.com/gui/home/upload).
- Now you trust me? Because I pointed you towards malware scanners? I could have engineered my malware just to avoid detection on what I advertised above. Don't trust me! 🤥
- Or you may just have a different Python or CUDA version, and the files here are not compatible anyway. Good, let's continue:

## How to compile & install bitsandbytes with CUDA yourself, on Windows.

Download and install:
- Microsoft Visual Studio [visualstudio.microsoft.com/downloads/](https://visualstudio.microsoft.com/downloads/)
- In MS Visual Studio installer, select "Desktop development with C++"
- CMake: [cmake.org/download/](https://cmake.org/download/)
- CUDA Toolkit: [developer.nvidia.com/cuda-downloads](https://developer.nvidia.com/cuda-downloads)
- In CUDA Toolkit installer, select 'with Visual Studio integration'.

Then, very important Windows absurdity (else compiling will fail!):
- Go to `C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v12.6\extras\visual_studio_integration\MSBuildExtensions`
- Copy what you find there (~4 files including a `Nvda.Build.CudaTasks.v12.6.dll`) to:
- `C:\Program Files (x86)\Microsoft Visual Studio\2019\Enterprise\MSBuild\Microsoft\VC\v170\BuildCustomizations`
- Replace `v12.6` or `v170` with whatever version you have, the above are just example default paths.

Finally:
- [bitsandbytes](https://github.com/bitsandbytes-foundation/bitsandbytes): `git clone https://github.com/bitsandbytes-foundation/bitsandbytes.git`
- cd bitsandbytes

Then:
```
pip install -r requirements-dev.txt
cmake -DCOMPUTE_BACKEND=cuda -S .
cmake --build . --config Release
pip install .
```
If something fails, perhaps entering this will fix it (make sure the path is correct):
- `set DCMAKE_GENERATOR_TOOLSET="cuda=C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v12.6"` [Enter], then retry the above.

After waiting around a bit, you should now find there's a compiled `bitsandbytes/libbitsandbytes_cuda126.dll`! Yay! Now you can make a neat tidy wheel you can keep safe, together with the .dll, to install without compiling everything again:

```
python setup.py bdist_wheel
```

Now there should be a `dist/bitsandbytes-0.45.1.dev0-cp310-cp310-win_amd64.whl' file.
- cd dist
- pip install bitsandbytes-0.45.1.dev0-cp310-cp310-win_amd64.whl
- Should result in: `Successfully installed bitsandbytes-0.45.1.dev0`
- Check `<your-python-install>/site-packages/bitsandbytes` and copy the `.dll` there, if needed.

Final note: Fortunately, [bitsandbytes README.MD](https://github.com/bitsandbytes-foundation/bitsandbytes) says: `Windows support is quite far along and is on its way as well`. So here's to hoping this repo will soon be obsolete. 🤗

