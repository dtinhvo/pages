---
layout: default
title: Math Example
--- 

# Summary
	ala  -   Constantin
> [!Var] Nomenclatures  
> $[\ .. ]_{C,f}:=$linearized ${}f{}$lux coefficient for cell ${}C{}$enter
> $[\ .. ]_{F,f}:=$linearized ${}f{}$lux for cell neighbor on face ${}F{}$  
> $[\ .. ]_{V,f}:=$*non-*linearized ${}f{}$lux (on source side)  
> TBD other flux terms

- example 
	$$	
		\frac{\partial{} \rho{} \phi{} }{\partial{} t}   + \nabla \cdot{} (\rho{} \mathbf{u} \phi{}) = \nabla \cdot{} ( \Gamma{} \nabla  \phi{}) + Q
	$$
-   Discretize
	-> integral form
	$$	
		\int\limits_{\Delta{}t} \int\limits_{V_{C}} \frac{\partial{} \rho{} \phi{}}{\partial{} t} dVdt
		+ \int\limits_{\Delta{}t} \int\limits_{V_{C}} \nabla \cdot{} (\rho{} \mathbf{u} \phi{}) dVdt
		= \int\limits_{\Delta{}t} \int\limits_{V_{C}} \nabla \cdot{} ( \Gamma{} \nabla  \phi{})  dVdt
		+ \int\limits_{\Delta{}t} \int\limits_{V_{C}} Q dVdt
	$$
- Divergence theorem 
	-> Some terms put on faces
	$$	
		\int\limits_{\Delta{}t} \int\limits_{V_{C}} \frac{\partial{} \rho{} \phi{}}{\partial{} t} dVdt
		+ \int\limits_{\Delta{}t} \oint\limits_{S_{C}} \cancel{\nabla \cdot{}} (\rho{} \mathbf{u} \phi{}) d\vec{ S }  dt
		= \int\limits_{\Delta{}t} \oint\limits_{S_{C}}  \cancel{\nabla \cdot{}} ( \Gamma{} \nabla  \phi{})  d\vec{ S }  dt
		+ \int\limits_{\Delta{}t} \int\limits_{V_{C}}  Q dVdt
	$$
- midpoint rule   / sum contributions
	assuming orthogonal mesh
	$$	
			\int\limits_{\Delta{}t}  \frac{\partial{} \rho{}_{C} \phi{}_{C}}{\partial{} t} V_{C}dt
		+ \int\limits_{\Delta{}t} \sum _{f}  (\rho{}_{f} \mathbf{u}_{f} \phi{}_{f}) \vec{ S }_{f}  dt
		= \int\limits_{\Delta{}t} \sum _{f} ( \Gamma{}_{f} (\nabla  \phi{})_{f})  \vec{ S }_f  dt
		+ \int\limits_{\Delta{}t}  Q V_{C}dt 
	$$
- implicit Euler
	$$
	    \frac{ \left( \partial{} \rho{}_{C} \phi{}_{C} \right)^{t}   - \left( \partial{} \rho{}_{C} \phi{}_{C} \right)^{t - \Delta{}t}    }  
	    {  \Delta{}t}V_{C}
		+ \sum _{f}  (\rho{}_{f} \mathbf{u}_{f} \phi{}_{f})^{t} \vec{ S }_{f}  \cancel{\Delta{}t}
		= \sum _{f} ( \Gamma{}_{f} (\nabla  \phi{})_{f})^{t}  \vec{ S }_{f}  \cancel{\Delta{}t}
		+   \left(Q V_{C}\right)^{t }\cancel{\Delta{}t }
	$$
- Flux Linearization
	$$	
	    \begin{align} 
	          & \left(
	    \underbrace{ \frac{\left(\rho{}_{C} \phi{}_{C}\right)^{t}   } {\Delta{}t}  V_{C}   }  
	    _{ F_C ^{tmp}  \phi{}_{C}^{t}}
	    \underbrace{ - \frac{ \left(  \rho{}_{C}\phi{}_{C} \right) ^{t - \Delta{}t}   }  
	    { \Delta{}t } V_{C}  }  
	    _{  F_{V}  ^{tmp}  }
	    \right)
	     + \left( \sum _{f} 
	    \underbrace{ \rho{}_{f} u_{f} \phi{}_{f} }  
	    _{ J_{f}^{conv}  = F_{c,f}^{conv} \phi{}_{C} +F_{f,f}^{conv} \phi{}_{F} +F_{V,f}^{conv}  }
	     \cdot{} \vec{ S }  _{f} \right) ^{t} \\
	      & = \left( \sum _{f} 
	    \underbrace{ \Gamma{}_{f} \left(\nabla  \phi{}_{f}\right)_{f} }  
	    _{ J_{f}^{diff}  = F_{c,f}^{diff} \phi{}_{C} +F_{f,f}^{diff} \phi{}_{F} +F_{V,f}^{diff} }
	     \cdot{} \vec{ S }  _{f} \right) ^{t}
	     + \left( 
	    \underbrace{ Q_{C} V_{C}  }  
	    _{ J_{C}^{src} =  F_{C}^{src} \phi{}_{C} + F_{V}^{src} }
	    \right) ^{t}
	    \end{align}
	$$
	- rewrite as grouped terms:
		$$
		    \begin{align} 
		          & \left(
		    F_C ^{tmp}  \phi{}_{C}^{t}
		     + F_{V}  ^{tmp}  
		    \right)
		     + \sum _{f} 
		     \left( F_{c,f}^{conv} \phi{}_{C} +F_{f,f}^{conv} \phi{}_{F} +F_{V,f}^{conv}\right)^{t}
		     \cdot{} \vec{ S }  _{f}  \\
		      & =  \sum _{f} 
		    \left(F_{c,f}^{diff} \phi{}_{C} +F_{f,f}^{diff} \phi{}_{F} +F_{V,f}^{diff}\right)^{t}
		     \cdot{} \vec{ S }  _{f} 
		     + \left( 
		    F_{C}^{src} \phi{}_{C} + F_{V}^{src} 
		    \right) ^{t}
		    \end{align}
		$$
- grouping implicit explicit
	$$	
	    \begin{align}
	     & \left. \begin{array} {l} 
	                \left(
	    F_C ^{tmp}  
	    + \sum _{f} \left( F_{c,f}^{conv} -F_{c,f}^{diff} \right)^{t} \cdot{} \vec{ S }  _{f} 
	    +F_{C}^{src}
	    \right) \phi{}_{C}^{t} \\
	       + \left( 
	     \sum _{f} \left( F_{f,f}^{conv}  - F_{f,f}^{diff}  \right)^{t}\cdot{} \vec{ S }  _{f} 
	     \right) \phi{}_{F}^{t} \\  
	    \end{array} \qquad \right\} \quad \text{implicit}  \\
	      & =   
	      - F_{V}  ^{tmp}     
	      + \sum _{f} \left(- F_{V,f}^{conv} +F_{V,f}^{diff}\right)^{t}
	     \cdot{} \vec{ S }  _{f} \} 
	     + F_{V}^{src,t} \qquad \} \quad \text{explicit} 
	    \end{align}
	$$
	signs of ${}F{}$ arbitrary, depends on govt. eq
### Boundary condition  
###### Summary of openFOAM implementation

| Operation \ Contribution | diagonals                | source                   |
| :----------------------- | :----------------------- | :----------------------- |
| divergence               | `valueInternalCoeffs`    | `valueBoundaryCoeffs`    |
| laplacian                | `gradientInternalCoeffs` | `gradientBoundaryCoeffs` |


| Function \             | fixedValue                            | fixedGradient                            | mixed                                                                                    |
| :--------------------- | :------------------------------------ | :--------------------------------------- | :--------------------------------------------------------------------------------------- |
| valueInternalCoeffs    | `0`                                   | `1`                                      | `(1.0 - valueFraction_)`                                                                 |
| valueBoundaryCoeffs    | `*this`                               | `gradient()/this->patch().deltaCoeffs()` | `valueFraction_*refValue_ + (1.0 - valueFraction_)*refGrad_/this->patch().deltaCoeffs()` |
| gradientInternalCoeffs | `-this->patch().deltaCoeffs()`        | `0`                                      | `-valueFraction_*this->patch().deltaCoeffs()`                                            |
| gradientBoundaryCoeffs | `this->patch().deltaCoeffs()*(*this)` | `gradient()`                             | `valueFraction_*this->patch().deltaCoeffs()*refValue_ + (1.0 - valueFraction_)*refGrad_` |

#### Dirichtlet
- Sufficient input is $\phi_{b}{}$   
	the rest provided by system 
##### Convective
- information transported from other (${}b{}$) cells
	$$
	    \begin{align}
		J_{b}^{conv} \cdot{}S_{b}  & =   
	    \underbrace{ 
	    \overbrace{ 0 }  
	    ^{ \text{\texttt{valueInternalCoeff}} }
	     }  
	    _{ F_{C,b} ^{conv}  }
	     \cdot{}\phi{}_{c} \qquad \qquad\qquad\qquad \} \quad \text{implicit} 
	      \\
	     &  \quad + \ \underbrace{ \rho{}_{b} u_{b} \cdot{}S_{b} 
	    \overbrace{ \phi{}_{b} }  
	    ^{ \text{\texttt{valueBoundaryCoeff}} }
	      }  
	    _{ F_{F,b}^{conv} } 
	     + 
	    \underbrace{ 0 }  
	    _{ F_{V,b}^{conv} } \qquad \} \quad \text{explicit} 
	    \end{align}
    $$
	- no local effect -> entirely explicit
##### Diffusive
- balance of in-and-out flux   %% details see diffu def and deriv %%
	$$	
	    \begin{align} 
	     	J_{b}^{diff} \cdot{}S_{b}  & =   
	    \underbrace{ \Gamma{}_{b} \cdot{}\lVert \mathbf{ S }  _{b} \rVert \cdot{} 
	    \overbrace{ - \frac{ 1 }     {\lVert \mathbf{ d }_{Cb}   \rVert }{} }  
	    ^{ \text{\texttt{gradientInternalCoeff = -deltaCoeffs()}} }
	      }  
	    _{ F_{C,b} ^{diff} }
	     \cdot{}\phi{}_{c}    \qquad\qquad\qquad\qquad \} \quad \text{implicit} 
	     \\
	     & \quad   \ \underbrace{ \Gamma{}_{b} \cdot{}\lVert \mathbf{ S }  _{b} \rVert \cdot{} 
	    \overbrace{ \frac{1}     {\lVert \mathbf{ d }_{Cb}   \rVert }{} \phi{}_{b}  }  
	    ^{ \text{\texttt{gradientBoundaryCoeff = deltaCoeffs() * (*this) }} }
	     }  
	    _{ F_{F,b}^{diff} }
	     + 
	    \underbrace{ 0 }  
	    _{ F_{V,b}^{diff} } \qquad \qquad \} \quad \text{explicit} 
	    \end{align}
	$$
	- split explicit and implicit
#### Neumann  
> [!theorem] Flux  
> $$    
> 	q_{b} =    
> 	\frac{\phi{}_{b} - \phi{}_{C}}    
> 	{\lVert d_{C,b} \rVert }
> $$
- Sufficient input is $q_{b}$  -> `gradient()` 
	the rest provided by system
##### Convective
- 
	$$
	    \begin{align} 
	     J_{b}^{conv} \cdot{}S_{b}  & =   
	     \underbrace{ \rho{}_{b}u_{b}\cdot{}S_{b} \cdot{} 
	    \overbrace{ 1 }  
	    ^{ \text{\texttt{valueInternalCoeff}} }
	     }  
	    _{ F_{C,b} ^{conv}  }
	     \cdot{}\phi{}_{c} \qquad\qquad\qquad\qquad\qquad\qquad\qquad \} \quad \text{implicit}  \\
	     & \quad + 
	    \underbrace{ 0 }  
	    _{ F_{F,b}^{conv} }
	       \phi{}_{b} 
	     + 
	    \underbrace{ \rho{}_{b} u _{b}  \cdot{} S_{b} \cdot{} 
	    \overbrace{ q_{b}\lVert d_{C,b} \rVert }  
	    ^{ \text{\texttt{valueBoundaryCoeff = gradient()/deltaCoeffs()}} }
	       }   
	    _{ F_{V,b}^{conv} } \qquad \} \quad \text{explicit} 
	    \end{align}
	$$
##### Diffusive
- 
	$$
	    \begin{align} 
	     J_{b}^{diff} \cdot{}S_{b}  & =   
	    \underbrace{
	    \overbrace{ 0 }  
	    ^{ \text{\texttt{gradientInternalCoeff}} }
	    }  
	    _{ F_{C,b} ^{diff}  } 
	     \cdot{}\phi{}_{c}  \qquad\qquad\qquad\qquad\qquad\qquad\qquad \} \quad \text{implicit} 
	      \\
	     & \quad +
	    \underbrace{ 0}  
	    _{ F_{F,b}^{diff} }
	       \phi{}_{b} 
	     + 
	    \underbrace{ \Gamma{}_{b} \cdot{}S_{b} \cdot{} 
	    \overbrace{ q_{b} }  
	    ^{ \text{\texttt{gradientBoundaryCoeff = gradient()}} }
	      }  
	    _{ F_{V,b}^{diff} }   \qquad\qquad \} \quad \text{explicit} 
	    \end{align}
	$$
#### Robin
- 
	$$
	    \underbrace{ \phi{}_{b} }  
	    _{ \text{\texttt{refValue\_}} }
	     = 
	    \underset{ \text{\texttt{valueFraction\_}} }{ \underbrace{ \omega{}  }  
	    \phi{}_{b} + (1-
	    \underbrace{ \omega{} }  
	    ) } \left( \phi{}_{C} + 
		    \underbrace{ \left(\frac{\partial{} \phi{}}{\partial{} x}\right)_{b} }  
		    _{ q_{b} \to{}\text{\texttt{refGrad\_}} }
		     \lVert d_{C,b} \rVert     \right)  
	$$
##### Convective
- 
	$$	
	    \begin{align}
		J_{b}^{conv} \cdot{}S_{b}  & =   
	    \underbrace{ 
	    \rho{}_{b}u_{b}\cdot{}S_{b}  \cdot{}
	    \overbrace{ (1-\omega{}) }  
	    ^{ \text{\texttt{valueInternalCoeff = valueFraction\_}} }
	     }  
	    _{ F_{C,b} ^{conv}  }
	     \cdot{}\phi{}_{c} \qquad\qquad\qquad\qquad\qquad\qquad\qquad \} \quad \text{implicit} 
	      \\
	     &  \quad + \ \overset{ \text{\texttt{valueBoundaryCoeff=valueFraction\_*refValue\_+(1-valueFraction\_)*refGrad\_/deltaCoeffs()}} }{\underbrace{ \rho{}_{b} u_{b} \cdot{}S_{b} 
	    \overbrace{ \omega{} \phi{}_{b} }  
	      }  
	    _{ F_{F,b}^{conv} }
	     + 
	    \underbrace{ \rho{}_{b} u _{b}  \cdot{} S_{b} \cdot{}
	    \overbrace{  (1-\omega{}) \cdot{}q_{b}\lVert d_{C,b} \rVert }  }  
	    _{ F_{V,b}^{conv} }} \qquad\} \quad \text{explicit} 
	    \end{align}
	$$
##### Diffusive
- 
	$$
	    \begin{align} 
	     J_{b}^{diff} \cdot{}S_{b}  & =   
	    \underbrace{
	    \Gamma{}_{b} \cdot{} \mathbf{ S }  _{b}  \cdot{} 
	    \overbrace{ - \frac{ 1 }     {\lVert \mathbf{ d }_{Cb}    \rVert } \cdot{} \omega{}{} }  ^{\text{\texttt{gradientInternalCoeffs:valueFraction\_*deltaCoeffs()}} }
	    }  
	    _{ F_{C,b} ^{diff}  }
	     \cdot{}\phi{}_{c}  \qquad\qquad\qquad\qquad \} \quad \text{implicit} 
	     \\
	     & \quad +\overset{ \text{\texttt{gradientBoundaryCoeff=valueFraction\_*deltaCoeffs()*refValue\_+(1-valueFraction\_)*refGrad\_ }} }{ 
	    \underbrace{ \Gamma{}_{b} \cdot{}\mathbf{ S }  _{b} \cdot{} 
	    \overbrace{ \frac{1}     {\lVert \mathbf{ d }_{Cb}   \rVert }{} \cdot{} \phi{}_{b} \cdot{}\omega{} }  
	     }  
	    _{ F_{F,b}^{diff} }
	     + 
	    \underbrace{ \Gamma{}_{b} \cdot{}S_{b} \cdot{} 
	    \overbrace{ (1-\omega{}) q_{b} }  
	      }  
	    _{ F_{V,b}^{diff} } } \qquad\} \quad \text{explicit} 
	    \end{align}
	$$
### Coupled BC
![|930x854](../../@Medias/2025-07-18-3.webp)
