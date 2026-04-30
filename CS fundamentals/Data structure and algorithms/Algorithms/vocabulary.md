
**Formulating the Problem** As in the discussion above, we are given a **set of**  
**points** P = {(x1, y1), (x2, y2), . . . , (xn, yn)}, with x1 < x2 < . . . < xn. We will use  
**pi to denote the point (xi, yi)**. We must first **partition P into some number**  
**of segments**. Each segment is a subset of P that represents a contiguous set  
of x-coordinates; that is, it is a subset of the form {pi, pi+1, . . . , pj-1, pj} for  
some indices i ≤ j. Then, for each segment S in our partition of P, we compute  
the line minimizing the error with respect to the points in S, according to the  
formulas above.

Experimentally, the best caching algorithms under this requirement seem to be  
variants of the Least-Recently-Used (LRU) Principle, which proposes evicting  
the item from the cache that was referenced longest ago