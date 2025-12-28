## Project: Locality
  Rotates a given .ppm image

## Course: Machine Structure and Assembly Language Programming (Spring '25)

**a2plain.c by Sara Abuhamra and Erica Huang**

**uarray2b.c by Erica Huang (and partly Sara Abuhamra -- details in header of uarray2b.c)**

**ppmtrans.c by Erica Huang**

## Acknowledgements: 
    TAs and Mark Sheldon

## Implementation: 
    i believe everything to be correctly implemented

## Architecture of solutions:
  ### uarray2b.c: 
        our UArray2b_T is a struct. The underlying representation
        of a UArray2b_T is a UArray2_T, where each element is a UArray_T. Each
        UArray_T represents a block within the UArray2b_T. We relied on the
        UArray2_T functionality as much as possible in out implementation. The
        most intricate part of this implementation is the mappin functions (
        Erica implemented this alone). There are 4 different mapping functions
        for the 4 different kinds of blocks, a middle/ full, right, bottom, or
        corner block. This is because right edge blocks coudl potentially miss
        elements on its right size, bottom blocks could potentially miss
        elements on its bottom side, and corner bocks coudl potentially miss
        elements on both is bottom and right side. This means that we have to
        implement different mapping functions for each scenario to avoid trying
        to access elements that don't exist. Another intricate part is the
        index conversion (from UArray2b_T to UArray_T). This conversion is used
        solely by out mapping function so we know where to access the UArray_T.

  ### a2plain.c: 
        This file simply implements a subclass of A2Methods_UArray2
        where the instance of this subclass is a UArray2_T. Therefore, we can
        solely rely on uarray2.c functionality, with little to implement
        ourselves.

  ### ppmtrans.c: 
        The way I did this was by creating a destination
        A2Methods_UArray2 to store the rotated image. The destination image
        either has the same dimensions as the original image (180 degree 
        rotation)or its width and height are switched (90 degree rotation). 
        I also created 2 different apply() functions for 90 and 180 degrees,
        becuase the pixel rotation math is different. When I use the map 
        function with the corresponding apply function, the apply function takes
        the current element of the original image and stores it in the
        transformed location of the destination image. After the destination
        image is created, I create a Pnm_ppm type where the pixels member is the
        destination image (because the destination image is a A2Methods_UAray2).
        Then I use Pnm_ppm functinality to print it. If the client wants to
        rotate it 0 degrees. I create a destination image still, but I just
        set it to point to the original image, so there is no real 
        transformation. No matter if the client specifies or not, I always time
        the rotation (or for the case of 0 degrees, I time how long it takes
        to set the destination image to point to the orginal image). Only if the
        client specifies "-time" do I write it to the specified file


## EXPERIMENT:
    Model name: Intel(R) Xeon(R) Silver 4214Y CPU @ 2.2
    Model: 85
    
    Note: r = row-major, c = col-major, b = block-major
            90 = 90 deg rotation, 180 = 180 deg rotation, 0 = 0 deg rotation
            ex. 90r = 90 degree rotation using row-major
    
    flowers	180x101		
    type	nanoseconds	per pixel (ns)	        instructions per pixel
    0r	702	        0.03861386139	        0.0386
    0c	848	        0.04664466447	        0.0466
    0b	966	        0.05313531353	        0.0531
    90r	719844	        39.59537954	        40
    180r	812158	        44.67315732	        45
    90c	861206	        47.37106711	        47
    180c	1377750	        75.78382838	        76
    90b	1643282	        90.38954895	        90
    180b	1754646	        96.51518152	        97
    
    
    mobo 0.5	4080x3060		
    type	nanoseconds	per pixel (ns)	        instructions per pixel
    0b	982	        0.00007865564526	0.0001
    0c	1048	        0.00008394207356	0.0001
    0r	1173	        0.00009395424837	0.0001
    180r	569611185	45.62437404	        46
    90c	1054372821	84.45251994	        84
    90r	1130287333	90.53307486	        91
    90b	1208137147	96.76864243	        97
    180b	1208379432	96.78804883	        97
    180c	1929861827	154.5769117	        155
    
    from wind cave 3.5	2856x1603		
    type	nanoseconds	per pixel (ns)	        instructions per pixel
    0r	902	        0.0001970220403	        0.0002
    0b	944	        0.0002061960155	        0.0002
    0c	1118	        0.0002442024845	        0.0002
    180r	208048012	45.44350753	        45
    90r	326163077	71.24314289	        71
    90c	355536073	77.65902715	        78
    90b	438312572	95.73973083	        96
    180b	446241649	97.47166312	        97
    180c	513499824	112.1627306	        112
    
    
    segfault 3	3414x2166		
    type	nanoseconds	per pixel (ns)	        instructions per pixel
    0c	1210	        0.0001636301774	        0.0002
    0r	1249	        0.0001689042079	        0.0002
    0b	1368	        0.0001849967626	        0.0002
    180r	342439004	46.30855783	        46
    90r	546487628	73.90236985	        74
    90c	578736169	78.26339009	        78
    90b	716932879	96.9519456	        97
    180b	719421461	97.28848041	        97
    180c	971190195	131.3355569	        131



## ANALYSIS:
    unsurprisingly, the 0degree rotations take the least amount of time total, 
    least amount of time per pixel, and the least amount of instructions
    per pixel. This is because in my code I am merely setting a pointer,
    and not actually reading or writing anything. 
    
    What I noticed with 90 and 180 rotations is that row major is faster than col
    and block in both 90 and 180 degrees, 90c is faster than both blocks, but 90b
    and 180b is faster than 180c. It was so surprise to be that 180r was the fastest
    out of the 90 and 180 degree rotations, becuase the way the UArray2 is set up,
    each row is it's out UArray_T. This means that when you pull and element of the
    row into the cache, you pull other elements from the row/UArray_T, giving it
    a high hit rate. 180r is faster than 90r because writing with 90r means you are 
    writing to a column, giving you a lower hit rate because each column is made up
    of multiple UArray_Ts. 180r however is writing to a row which is the same high
    hit rate as reading it. I was surprised that a column major made it above the
    blocked majors, because i expected bock major to act the same as row major 
    because each block is also its own UArrayT, meaning that reading from a block 
    in a UArray2b_T should have the same high hit rate as reading a row from a
    UArray2_T. Its no surprise that 180c was the slowest because it is both reading
    and writing from columns, which is multiple UArray2_Ts, so it will have to 
    pull from main memory more often.
    
    This data was all pretty consistent with the bigger images, but the flowers
    image was an outlier, probably because of its small size (it was probably
    trivial)


## Time Analysis
    Spent approximately 40 hours on this assignment.
