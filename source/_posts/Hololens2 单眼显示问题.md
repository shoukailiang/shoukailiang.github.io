---
title: Hololens2 单眼显示问题
date: 2022-08-22 16:09:02
tags: 
  - hololens2
  - unity
categories:
 - hololens2
---

# 法1：使用mrtk自带的shader
使用MRTK中自带的shader
![请添加图片描述](https://img-blog.csdnimg.cn/eb23ebfb6ee6489ab7b5803a9e1bbb76.png)

# 法2：自定义shader
- a2v 中添加 UNITY_VERTEX_INPUT_INSTANCE_ID

- v2f 中添加 UNITY_VERTEX_OUTPUT_STEREO

- vert 中添加 

  ​		UNITY_SETUP_INSTANCE_ID(v); //Insert
  ​        UNITY_INITIALIZE_OUTPUT(v2f, o); //Insert
  ​        UNITY_INITIALIZE_VERTEX_OUTPUT_STEREO(o); //Insert
  ## demo

```bash
Shader "JaffHan/Wireframe" {
Properties {
        _Color("Color",Color)=(1.0,1.0,1.0,1.0)
        _EdgeColor("Edge Color",Color)=(1.0,1.0,1.0,1.0)
        _Width("Width",Range(0,1))=0.2
    }
SubShader {

    Pass {
    CGPROGRAM
    #pragma vertex vert
    #pragma fragment frag
    #pragma target 3.0
    #include "UnityCG.cginc"
    

    struct a2v {
        half4 uv : TEXCOORD0 ;
        half4 vertex : POSITION ;
        UNITY_VERTEX_INPUT_INSTANCE_ID //Insert
    };

    struct v2f{
        half4 pos : SV_POSITION ;
        half4 uv : TEXCOORD0  ;
        UNITY_VERTEX_OUTPUT_STEREO //Insert
    };
    fixed4 _Color;
    fixed4 _EdgeColor;
    float _Width;
    
    v2f vert(a2v v)
    {
        v2f o;
        UNITY_SETUP_INSTANCE_ID(v); //Insert
        UNITY_INITIALIZE_OUTPUT(v2f, o); //Insert
        UNITY_INITIALIZE_VERTEX_OUTPUT_STEREO(o); //Insert
        o.uv = v.uv;
        o.pos=UnityObjectToClipPos(v.vertex);
        return o;
    }


    fixed4 frag(v2f i) : COLOR
    {
        fixed4 col;
        float lx = step(_Width, i.uv.x);
        float ly = step(_Width, i.uv.y);
        float hx = step(i.uv.x, 1.0 - _Width);
        float hy = step(i.uv.y, 1.0 - _Width);
        col = lerp(_EdgeColor, _Color, lx*ly*hx*hy);
        return col;
    }
    ENDCG
    }
} 
    FallBack "Diffuse"
}
```
