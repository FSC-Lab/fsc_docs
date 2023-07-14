Mathematical Notations
======================

Typeface
--------

.. table::
   :widths: auto

   ========================================= ==========================================================================================================
    Symbol                                    Meaning                                                                                                  
   ========================================= ==========================================================================================================
    :math:`a`                                 An italic lowercase letter denotes a real scalar value                                                   
    :math:`\mathbf{a}`                        A bold lowercase letter denotes a real column vector                                                     
    :math:`\mathbf{A}`                        A bold uppercase letter denotes a real matrix                                                            
    :math:`\underrightarrow{a}`               A lower calligraphic letter an under-arrow denotes a physical vector                                     
    :math:`\underrightarrow{\mathcal{F}}_a`   A uppercase calligraphic F with an under-arrow denotes a reference frame                                 
    :math:`\mathcal{C}`                       Consider using uppercase calligraphic letters to denote sets or groups that you introduce anew           
    :math:`\mathtt{A}`                        Consider using the typewriter font to denote artificially constructed quantities with no meaningful name 
   ========================================= ==========================================================================================================

Common Symbols
--------------
.. table::
   :widths: auto

   ======================================================================================================================== ================================================================================
    Symbol                                                                                                                   Meaning                                                                        
   ======================================================================================================================== ================================================================================ 
    :math:`\emptyset`                                                                                                        The empty set                                                                  
    :math:`\mathbb{Z}`                                                                                                       The set of integers                                                            
    :math:`\mathbb{Z}_{\geq 0}`                                                                                              The set of nonnegative integers                                                
    :math:`\mathbb{N}`                                                                                                       The set of natural numbers                                                     
    :math:`\mathbb{R}`                                                                                                       The set of real numbers                                                        
    :math:`\begin{aligned} \mathbb{R}_{> 0}&, \mathbb{R}_{\geq 0}, \\ \mathbb{R}_{< 0}&, \mathbb{R}_{\leq 0}\end{aligned}`   The set of positive, nonnegative, negative, and nonpositive real numbers       
    :math:`\mathbb{R}^n`                                                                                                     The set of n-tuples or n-vectors                                               
    :math:`\mathbb{R}^{m\times n}`                                                                                           The set of m-by-n matrices                                                     
    :math:`\mathbb{H}`                                                                                                       The set of quaternions                                                         
    :math:`\mathcal{S}^3`                                                                                                    The 3-sphere, or the set of unit quaternions                                   
    :math:`SO(3)`                                                                                                            The Special Orthogonal Group (in 3 dimensions), representing spatial rotations 
    :math:`\mathfrak{so}(3)`                                                                                                 The Lie algebra associated with :math:`SO(3)`                                  
    :math:`SE(3)`                                                                                                            The Special Euclidean Group (in 3 dimensions), representing poses              
    :math:`\mathfrak{se}(3)`                                                                                                 The Lie algebra associated with :math:`SE(3)`                                  
    :math:`\mathbf{0}_{m \times n}`                                                                                          The zero matrix (size subscript optional)                                      
    :math:`\mathbf{1}`                                                                                                       The identity matrix                                                            
    :math:`\mathbf{1}_n`                                                                                                     The :math:`n`\ :sup:`th` standard basis vector                                 
   ======================================================================================================================== ================================================================================

Super/Subscripts and accents
----------------------------

.. table::

   ================================ =================================================================================================================
   Symbol                           Meaning
   ================================ =================================================================================================================
   :math:`\hat{\mathbf{a}}`         Indicates an estimate of :math:`\mathbf{a}`                                                          
   :math:`\tilde{\mathbf{a}}`       The error in estimating :math:`\mathbf{a}`, usually defined as :math:`\hat{\mathbf{a}} -\mathbf{a}`
   :math:`\mathbf{a}_k`             The value of a :math:`\mathbf{a}` at a timestep. Timesteps are always labelled :math:`k` but may be ornamented
   :math:`\mathbf{a}^i`             The value of a :math:`\mathbf{a}` at some other index :math:`i`. Prefer :math:`i,j` for counting indices
   :math:`\mathbf{a}_i`             Element :math:`i` of vector :math:`\mathbf{a}`
   :math:`\mathbf{a}_{i:j}`         A slice with dimension :math:`\mathbb{R}^{j-i+1}` of vector :math:`\mathbf{a}`
   :math:`\mathbf{A}_{ij}`          Element :math:`ij` of matrix :math:`\mathbf{A}`
   :math:`\mathbf{A}_{(p:q, r:s)}`  A block with dimension :math:`\mathbb{R}^{q-p+1 \times r-s+1}` of matrix :math:`\mathbf{A}`
   :math:`\mathbf{A}^\top`          Transpose of :math:`\mathbf{A}`
   :math:`\mathbf{a}^\times`        Skew-symmetric "cross product matrix" formed from 3-vector :math:`\mathbf{a}`
   :math:`\mathbf{A}^\vee`          Inverse of :math:`\mathbf{a}^\times` to recover :math:`\mathbf{a}` from the skew-symmetric matrix
   :math:`\mathbf{a}^\wedge`        Operator mapping :math:`\mathbf{a}` to an element of some Lie algebra whose vector space this vector resides in
   :math:`\mathbf{a}^\mathcal{I}`   A purely imaginary quaternion with imaginary part filled by elements of 3-vector :math:`\mathbf{a}`
   ================================ =================================================================================================================

Operators and functions
-----------------------

.. table::

   ========================================================== ===========================================================================================================
   Symbol                                                     Meaning 
   ========================================================== ===========================================================================================================
   :math:`\lvert a \rvert`                                    Absolute value of a scalar :math:`A`
   :math:`\lVert \mathbf{a} \rVert`                           Eucliean norm of a vector :math:`\mathbf{a}`
   :math:`\lVert \mathbf{a} \rVert_p`                         :math:`\ell^p` norm of a vector :math:`\mathbf{a}`
   :math:`\lVert \mathbf{a} \rVert_\infty`                    :math:`\ell^\infty` norm (maximum absolute value) of a vector :math:`\mathbf{a}`
   :math:`\lambda_\mathrm{max}(\mathbf{A})`                   Maximum Eigenvalue of matrix :math:`\mathbf{A}`
   :math:`\lambda_\mathrm{min}(\mathbf{A})`                   Minimum Eigenvalue of matrix :math:`\mathbf{A}`
   :math:`\boldsymbol{\sigma}_\mathrm{max}^*(\mathbf{A})`     Maximum singular value of matrix :math:`\mathbf{A}`
   :math:`\boldsymbol{\sigma}_\mathrm{min}^*(\mathbf{A})`     Minimum singular value of matrix :math:`\mathbf{A}`
   :math:`\operatorname{tr}(\mathbf{A})`                      Trace of matrix :math:`\mathbf{A}`
   :math:`\det(\mathbf{A})`                                   Determinant of matrix :math:`\mathbf{A}`
   :math:`\operatorname{diag}(\mathbf{A},\ldots,\mathbf{Z})`  A block diagonal matrices consisting of diagonal submatrices :math:`\mathbf{A}` to :math:`\mathbf{Z}`
   :math:`\operatorname{diag}(a,\ldots,z)`                    A diagonal matrices consisting of diagonal elements :math:`a` to :math:`z`
   :math:`D_\mathbf{x}\mathbf{f}`                             Derivative of function :math:`\mathbf{f}` w.r.t. :math:`\mathbf{x}`
   :math:`D^n_\mathbf{x}\mathbf{f}`                           :math:`n`-th derivative of function :math:`\mathbf{f}` w.r.t. :math:`\mathbf{x}`
   :math:`D_\mathbf{y}D_\mathbf{x}\mathbf{f}`                 A mixed derivative of function :math:`\mathbf{f}` w.r.t. :math:`\mathbf{x}` and :math:`\mathbf{y}`
   :math:`L_\mathbf{f}^n\mathbf{g}`                           :math:`n`-th order Lie derivative of function :math:`\mathbf{g}` along the vector field :math:`\mathbf{f}`
   :math:`\mathbf{f} \circ \mathbf{g}`                        A function that is the composition of :math:`\mathbf{f}` and :math:`\mathbf{g}`
   ========================================================== ===========================================================================================================
