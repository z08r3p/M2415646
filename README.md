java c
M2 Project Progress Report - Virtual Hair Style. Fit 
October   17, 2024
1          Project Objective 
As Figure 1 shows, this   project   aims   to   change   hair   color   with   hairstyle   fitting,   including:
• Shape Transform.: Transform. the hair outline   in   target   images   to   fit   with   reference   im-   ages as precisely as possible.
• Color Change: Map   hair   color   into   expectation   directly.
                                                                                                                
Figure   1:   Shape   Transform.
2 Entities and Relationships 
So   far, we   have   identified   the   following   components:
• Element: Pixels of images, atomic units processed for desired appearance.
• Entity: All images used in the   project   are   collections   of entities.    Besides the   reference   image, a target image is necessary to transform. the hair   shape.
• Attribute: Positions   and   colors   of   pixels   in   images   are   attributes   we   focus   on, especially hair part(it meas that we   supposed we have   identified   the   position   of hair   in   images).   In   an   image,   a   pixel   has   a   value   to   represent   the   color,   and   pixels   with   specified   positions   can make up the outline and   shape of contents.
Let us draw a conclusion about relationships among attributes,   elements   and   entities.

Figure   2:   Entity   Relation
In summary, the entities and elements in this problem   are   followed:
•    The reference image   R hasn pixels for hair   part.    The pixel   at position   i with   value   Ri   ,   for   i   =   1,...,n.
•    Supposed   we   have   already   identified   the   hair   positions.    Therefore,   the   target   image   T   has   m   pixels   constituting   hairs.   The   pixel   at   position   j   with   value   Tj   , for   j   =   1,...,n.
•    The resultant image   R*    has   same   n pixels   with   origin   one.    The pixel   at position   i with   content   Ri(*), for   i   =   1,...,n.
The relationship between the entities   and elements has following points:
•    Pixels   at   non-hair positions   should keep unchanged   as possible,   like   human   face,   back-   ground.   However, it   is   possible   to   change   these   parts   little   for   better   fit.
• Give   known   hair   position   of   images, hair   pixels   are   primary   elements   we   focus   on.
Generally,   we   can   abstract   this   task   as   a   function   F to   transform. pixels   of hair parts   in   target   image to fit with reference image for the least   error:

where i is same position of   pixels among reference and resultant images
3 Problem Definition 
• Objective: Given   a   target   image   with   known   position   pixels   of   hairs, our   design   F   trans-   forms hair pixels to fit with reference image with least error.
• Inputs: 
– Reference image R: Contains   n   pixels, where   each   pixel   at   position   i   has   value   Ri   ,   for   j   =   1,...,   n.
–    Target image T: Contains   n   pixels   for   hair   part,   where   pixel   at   position   j   with   value   Tj   , for   j   =   1,...,n.
• Outputs: 
You seem to assume correspondence   is   known.   If so, you can't   have   known correspondence   between any   image   pixel, You can only   have   known correspondence between some   identified landmark   points. Where are these landmark   points?
–    Transform. function F : F represents the   transformation including shape and color   change:
Rj(*)   = F(Rj   ) = C(T(Rj   ))                                                                                                                  (2)
T is shape transformation function, and C is   color   change function.
– Resultant target image R* : The   transformed   image   that   displays   the   modified   hair
on   the   target   image, it   contains   n   pixels, each   is   transformed   by   Rj(*)   = F(Rj   )
• Optimization Objective: Minimize the   difference between the resultant   image   and   the   origin image:
Minimize            ||F(Ri   ) - Ri   ||   2                                                                                                                                                                                       (3)
• Constraints:
–    Shape Constraint: The constraint using landmark points ensure   that hair   does   not   extend   below   the   top   of   the   eyebrows   or   beyond   the   sides   of   the   face.
–    Color Consistency Constraint: Average   color   α is used to   ensure   color   transfor-   mations maintain consistency   with surrounding pixels.
4 Algorithm Design 
Algorithm 1 Hair   Style   Fit
1:    Initialize   R*    =   R,   i.e.,   set   each   pixel   Ri(*)   =   Ri      for   all   i
2:    Initialize   error   E   =   ∞,   threshold   τ   >   0
3: Identify landmark points in R* : 
•    Set   leyebrow top    as   the   highest   pixel   point   where   hair   should   not   extend   below   (e.g.,   above   the   代 写M2 Project Progress Report - Virtual Hair Style Fit 2024
代做程序编程语言eyebrows).
•    Set   lface left    and   lface right    as   the   outermost   pixels   on   the   left   and   right   sides   of the   face, respectively.
•    Map corresponding landmark points between T and R.
4: Apply Thin Plate Spline (TPS) Transformation: 
•    Utilize TPS to perform. a smooth transformation of T based   on the mapped landmark   points.
•    Adjust   T   to   achieve   a   natural   transition   towards   the   reference   hair   shape   in   R*   .
5: Smooth and refine T: 
• Use   the   TPS   transformed   positions   to   further   smooth   the   edges   of   the   hair   region.
•    Adjust details to correct potential artifacts introduced by transformation.
6: Adjust hair from T to fit the reference image: 
7: *Note: This   step involves the correction   of hair position to   avoid   unnatural   breaks   and   to   ensure   it   aligns   correctly   with   the   facial   landmarks, as   illustrated   in   Figure.   3.
8: for each   pixel   pi   in   T do 
9: if pixel pi   is below leyebrow top then 
10:                                                 Trim   T   by   removing   pixels   below   leyebrow top
11: end if 
12: if pixel   pi   is beyond lface   left or lface right then 
13:                                              Trim T by adjusting pixels beyond lface   left or lface right
14: end if 
15: end for 
16: Color Adjustment of Hair: 
• Compute   the   average   color   of   hair   in   R*    as   CR      = AverageColor(R*   ).
•    Compute the   average   color   of target   color(the   color   which   we   want to transform   to)   as   CT      = AverageColor(T).
•    Calculate the scaling factors for color adjustment:
– scaler      = CR(*)/CT      for   each   color   channel   (e.g., RGB).
•    Apply   pi      = pi      × scaler   on   each   pixels   pi   in   T:
17: Global Transformation: 
18:      Initialize   transformation   parameters:   scales   =   1, rotation   θ   = 0, translation   t   =   [0,   0]
19: repeat 
20:                              Set   E′ =   E
21: for each   pixel   pi   in   T do 
22:                                                 Set   d   =   ∞
23: for each   pixel   qj      in   R* do 
24: if ||qj      −   pi   ||   < d then 
25:                                                                                              Set   c(pi   )   =   qj   ,   d   =   ||qj      −   pi   ||
26: end if 
27: end for 
28:                                                 Compute   local   transformation   Ti      from   pi   to   c(pi   )
29: Update global transformation: 
s =   s + α   × (scale   of   Ti      − s)
θ   = θ + α   ×   (rotation   of   Ti      − θ)
t = t + α   × (translation   of   Ti      −   t)
30:                                                 where   α is   an   coefficient   to   adjust   the   step   (e.g.,   0.1)
31: end for 
32:                         Apply   global   transformation   to   T:   T′ = Transform(T,s,θ,   t)
33:                              Compute   new   error   E   = Error(T′ ,   R)
34: until |E − E′ |   < τ
35:      Output   final   transformed   hair   mask   T′ 
36:      Overlay   T′ onto   R*    to   generate   final   image

Figure 3:   Landmark Points
5          Justification 
5.1 Correctness 
The correctness of the algorithm can   be   justified in the following aspects:
• Landmark Detection: The    algorithm   identifies   facial   landmarks,    like   the   top   of   the   eyebrows and the edges of the face,   to   enforce   constraints   that hair   cannot   extend   above   these points.   This prevents the hairstyle. from obscuring the face   and maintains   a   natural   appearance.      If hair   from   the   target   image   falls   below   the   eyebrow   line,   the   algorithm   trims it for a more visually   appealing look.
• Transformation: TPS ensures   a   seamless   transformation   and   natural   appearance   after   changing hairstyle.
• Color Change: During   the   color-changing   step, the   algorithm   calculates   the   average   hair color   from   both   images   and   applies   scaling   factors, ensuring   the   final   image   looks   natural   and visually appealing.
How does the algorithm satisfies optimization objective and   constraints?
5.2 Convergence 
The convergence of the algorithm is established through iteration and error minimization:
• Error Reduction: The algorithm initializes   an error metric   E to   quantify   the   difference   between   the   transformed   hairstyle   and   the   reference   hairstyle. In   each   iteration, it   updates the hair pixels to minimize the error until it sufficiently converges to the   threshold.
• Finite Iterations: The iterations are finite because the algorithm alters a limited number   of   pixels   within   defined   masks,   adhering   to   constraints   set   by   facial   landmarks.      This   ensures that the algorithm converges to a solution instead   of entering   an infinite   loop.



         
加QQ：99515681  WX：codinghelp  Email: 99515681@qq.com
