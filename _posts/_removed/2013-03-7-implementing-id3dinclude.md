---
layout: post
title: Implementing ID3DInclude
image: thumb_directx.png
comments: true
---
For the procedural methods coursework I am currently working on I wrote a small framework to hide away the DirectX API completely and make it easier to use and simpler to prototype with. The procedural real-time terrain generation is running on the GPU and therefore heavily relying on Shaders. 

To keep everything lightweight I wanted to depend on the minimum on dependencies, so mainly DirectX 11 obviously, but also the nice header-only math library GLM. At some point I encountered the problem that the Shaders were getting more complex and therefore I wanted to split the code in multiple files (especially the noise and fractal functions) to make them easier to maintain and reuse. To allow shader files that get compiled on runtime to include files the ID3DInclude-Interface has to be implemented. I stumbled upon a nice article by Adam Sawicki, that helped me a lot. Since his code was depending on his own framework, I thought I share the no-dependencies version of a ID3DInclude-Interface implementation.

Header-file:
{% highlight cpp %}
class CShaderInclude : public ID3DInclude {
public:
    CShaderInclude(const char* shaderDir, const char* systemDir) : m_ShaderDir(shaderDir), m_SystemDir(systemDir) {}
 
    HRESULT __stdcall Open(D3D_INCLUDE_TYPE IncludeType, LPCSTR pFileName, LPCVOID pParentData, LPCVOID *ppData, UINT *pBytes);
    HRESULT __stdcall Close(LPCVOID pData);
private:
    std::string m_ShaderDir;
    std::string m_SystemDir;
};
{% endhighlight %}

Source-file:
{% highlight cpp %}
const char* gSystemDir = "..\\Shader";
 
HRESULT __stdcall CShaderInclude::Open(D3D_INCLUDE_TYPE IncludeType, LPCSTR pFileName, LPCVOID pParentData, LPCVOID *ppData, UINT *pBytes) {
    try {
        std::string finalPath;
        switch(IncludeType) {
        case D3D_INCLUDE_LOCAL:
            finalPath = m_ShaderDir + "\\" + pFileName;
            break;
        case D3D_INCLUDE_SYSTEM:
            finalPath = m_SystemDir + "\\" + pFileName;
            break;
        default:
            assert(0);
        }
 
        std::ifstream includeFile(finalPath.c_str(), std::ios::in|std::ios::binary|std::ios::ate);
 
        if (includeFile.is_open()) {
            long long fileSize = includeFile.tellg();
            char* buf = new char[fileSize];
            includeFile.seekg (0, std::ios::beg);
            includeFile.read (buf, fileSize);
            includeFile.close();
            *ppData = buf;
            *pBytes = fileSize;
        } else {
            return E_FAIL;
        }
        return S_OK;
    }
    catch(std::exception& e) {
        return E_FAIL;
    }
}
 
HRESULT __stdcall CShaderInclude::Close(LPCVOID pData) {
    char* buf = (char*)pData;
    delete[] buf;
    return S_OK;
}
{% endhighlight %}