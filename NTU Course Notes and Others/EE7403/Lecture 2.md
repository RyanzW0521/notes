# NOTES OF EE7403
____

## lecture 1

- new words
- lecture notes

  ### new words
  #### __chapter 1__ 
  - perception 感知 dominating 占优势的 restoration 恢复 Morphological 形态学 clustering 聚类 Hough Transform 霍夫变换 histogram 直方图 graphic 形象的 reflectivity 反射 transmissivity 透光度
  intensity 强度 intervals 间隔 resolution 分辨率 cones 圆锥状 perceive 感知 cornea 角膜 subtractive 减性 cyan 青色 magenta 品红 hardware oriented 面向硬件 hue 色调 saturation 饱和度 intensive 强度 contrast 对比度 bimodal 双峰
  deblurring 去模糊 retouching 润饰 watermaking 水印 tampering 篡改 
  #### __chapter 2 LSI systems and Transforms__
  - rbitrary 任意的 commutative 交换的 complex function 复数方程 conjugate 共轭 separability 可分性 bounded 有界的 replication 复制 reciprocal 倒数 simultaneously 同时地 iterative 迭代  


## lecture 2 

- new words
- lecture notes

  ### Lecture notes
  #### __chapter 2__
   
    1. LSI system & Transforms
        
        1. Fourier Transform
        
            $ F(u) = F{f(x)} = \int f(x)exp(-j2 \pi ux)dx $
            
            
            $ f(x) = f^{-1}{F(x)} = \int F(u)exp(j2 \pi ux)du $ 
            
            2-dimentional:
            
            $ F(u,v) = F{f(x,y)} = \int \int f(x,y) e^{-j2 \pi (ux+vy)}dxdy $
            
            
            u and v are 2 frequency variables
            
            properties: 
            1. $ F(u,v) = \int F~x(u,y)e^{-j2 \pi vy}dy $ --------------separable
                only if $ f(x,y) = f~1(x)f~2(y) $, $ F(u,v) = f~1(x)f~2(y) $
            2. $ F(u,v) = R(u,v) + jI(u,v) = \vert {F(u,v)} \vert e^{j \psi (u,v)} $
                ---------complex function $ \phi (u,v) = tan^{-1} [ I(u,v)/R(u,v) ] $
                
         2. DFT 
             
             for a img with size(m \* n):
             
             $ F(u,v) = \frac{1}{mn} \sum^{m-1}_{x=0} \sum^{n-1}_{y=0} f(x,y)e^{[-j2\pi(ux/m+vy/n)]} $
             
             it is also separable. can get the 2-dimentional expression by row-column decomposition
             
             properties:
             1. periodicity: $ F(u,v) = F(u+m, v) = F(u, v+n) = F(u+m, v+n) $
             2. conjugate symmetry: $ F(u,v) = F^{*} (-u, -v),  \vert F(u,v) \vert = \vert F(-u,-v) \vert $
             3. linear and scaling: $$ F \{ \alpha f_1(x,y) + \beta f_2(x,y) + \cdots \}  =  \alpha F_1(u,y) + \beta F_2(u,v)+ \cdots + F\{ f(\alpha x, \beta y)\}  =   \frac{1}{\vert \alpha \beta \vert} F(u/\alpha, y/\beta) $$
             4. convolution theorem: 外项积： $ f(x,y) * g(x,y) <=> F(u,v) G(u,v) $  内向的就是反过来
                 
                 be careful to apply this in digital img-----apply 0 padding
                 
             5. translation
             6. rotation 
             7. __Rotation Invariant Transform__：$ g(u,v) = \frac{1}{\pi} \int_0^{2\pi} \int_0^1 e^{-j(2\pi ur^2 +v \theta} f(r,\theta)drd\theta $
                    
         3.  DCT 详见6427的笔记
         4. image sampling
             
             digital img ----> sampling ---->convert continous variable f_c into finite set of nums f_d ----> quantization ----> proper num range
             1. sampling: $ f_d(m,n) = f_c(m\Delta x, n \Delta y) = f_c(x,y)\vert_{Let x = m \Delta x, y=\Delta y} $
             2. process:
                 
                 $ sampling function: s(x,y) = \sum_{m=-\infty}^{\infty} \sum_{n=-\infty}^{\infty} \delta(x-m\Delta x, y-\Delta y) $
             
                 
                 $ FT: S(u,v) = \frac{1}{\Delta x \Delta y} \sum_{m=-\infty}^{\infty} \sum_{n=-\infty}^{\infty} \delta(\frac{u-m}{\Delta x}, \frac{v-n}{\Delta y}) $
               
                   
                 $ sampling img = f_d(x,y) = f_c(x,y)s(x,y) = \sum_{m=-\infty}^{\infty} \sum_{n=-\infty}^{\infty} f_c(m\Delta x, n\Delta y)\delta(x-m\Delta x, y-\Delta y) $
             
                 $$ 
                 FT of f_d: F_d(u,v) = F_c(u,v)*S(u,v) = \frac{\Delta x \Delta y} \sum_{m=-\infty}^{\infty} \sum_{n=-\infty}^{\infty} F_c(u,v) \delta(x-m\Delta x, y-\Delta y)
                                                       = \frac{\Delta x \Delta y} \sum_{m=-\infty}^{\infty} \sum_{n=-\infty}^{\infty} F_c(x-m\Delta x, y-\Delta y)
                                                       = \frac{\Delta x \Delta y} \sum_{m=-\infty}^{\infty} \sum_{n=-\infty}^{\infty} F_c(u-mf_{xs}, y-nf_{ys})
                 
                 $$
                 a perodic replication of Fc(u,v)
              3. if x, y smpling freq > bandwidths---->Fc(u,v)can be recovered by __a low pass filter__ with freq response $ H(u,v) = \{ _0^{\Delta x \Delta y} $
              
           
       5. img quantization     
       
           common L-level: $ f_q = Q(f) = r_k if t_k \le f < t_{k+1} for k = 1, \cdots , L $
           
           * The choice of quantized levels should minimize the quantization error between f and fq
           
           ![quantization 1](https://github.com/RyanzW0521/notes/blob/main/NTU%20Course%20Notes%20and%20Others/EE7403/IMGS/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202023-01-18%20165052.png)
           ![quantization 2](https://github.com/RyanzW0521/notes/blob/main/NTU%20Course%20Notes%20and%20Others/EE7403/IMGS/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202023-01-18%20165106.png)
           
           
           

