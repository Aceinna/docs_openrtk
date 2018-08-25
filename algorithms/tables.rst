.. table:: **My table**

    +---------+---------+
    |  logo1  |  logo2  |
    +---------+---------+
    |  logo1  |  logo2  |
    +---------+---------+
   

test
=====

.. tabularcolumns:: |p{1cm}|p{7cm}|p{17cm}|

+------------+------------+-----------+
| Header 1   | Header 2   | Header 3  |
+============+============+===========+
| body row 1 | column 2   | column 3  |
+------------+------------+-----------+
| body row 2 | Cells may span columns.|
+------------+------------+-----------+
| body row 3 | Cells may  | - Cells   |
+------------+ span rows. | - contain |
| body row 4 |            | - blocks. |
+------------+------------+-----------+




.. list-table:: **Another attempt to format a table**
   :header-rows: 1
   :widths: 6 5

   * - Header A1
     - Header B1

   * - A1
     - B1
     
     
     
     
======  =====  ======
   Inputs      Output
-------------  ------
  A       B    A or B
======  =====  ======
False   False  False
 True   False   True
False   True    True
 True   True    True
======  =====  ======



.. list-table:: **what?**
   :header-rows: 1
   :widths: 5 15 10

   * - Treat
     - Quantity
     - Description
       
   * - Albatross!
     - 299
     - On a stick!
       
   * - Crunchy Frog!
     - 1499
     - If we took the bones out, it wouldn't \\
       be crunchy, now would it?
             
             


.. tabularcolumns:: |l|c|r|

.. list-table:: test table #1
    :widths: 25 25 50
    :header-rows: 1

    * - Index
      - Time
      - Output
      
    * - 1
      - 0.0
      - -0.2
      
    * - 2
      - 0.1
      - -0.25
      
    * - 3
      - 0.2
      - -0.10
      
    * - 4
      - 0.3
      - -0.01
      
    * - 5
      - 0.4
      - 0.12


Table
=====



.. tabularcolumns:: |c||r|

=====  =====
col 1  col 2
=====  =====
1      Second column of row 1.
2      Second column of row 2.
       Second line of paragraph.
3      - Second column of row 3.

       - Second item in bullet
         list (row 3, column 2).
\      Row 4; column 1 will be empty.
=====  =====


.. tabularcolumns:: |l|c|r|r|

====  ===  ====  ==
aAee  bB   bB    bB
cC    edD  dD    dD
====  ===  ====  ==


.. tabularcolumns::
      |p{0.80\linewidth}c|p{0.10\linewidth}l|p{0.30\linewidth}r|
      
====  ===  ====
aAee  bB   bB  
cC    edD  dD  
====  ===  ====
      

.. tabularcolumns:: |l|l|l|
      
====  ===  ====
aAee  bB   bB  
cC    edD  dD  
====  ===  ====

    
.. table:: **GPS Messaging**

    +------------+-----------------------+----------------------------------+-------------+
    | **System** | **Message**           | **Description**                  | **Units**   |
    +============+=======================+==================================+=============+
    | NovAtel    | BESTVEL               || Actual direction of motion over | [deg]       |
    |            |                       || ground (track over ground) with |             |
    |            |                       || respect to True North           |             |
    +------------+-----------------------+----------------------------------+-------------+
    | NMEA       | VTG                   | True track made good             | [deg]       |
    +------------+-----------------------+----------------------------------+-------------+
    | SiRF       || Geodetic Navigation  || Course Over Ground              | [deg x 100] |
    |            || Data – Message ID 41 || (COG, True)                     |             |
    +------------+-----------------------+----------------------------------+-------------+


.. tabularcolumns:: |r|r|
.. csv-table::    
   :header: x, x*x
   3,9
   4,16
   9,81    
   10,100
   

.. tabularcolumns:: |r|r|
.. table:: **Truth table for "not"**

    =====  =====
      A    not A
    =====  =====
    False  True
    True   False
    =====  =====


.. centered:: **Centered words**


.. table:: Optional Caption
    :widths: 3 2 1
    :column-alignment: left center right
    :column-wrapping: true true false
    :column-dividers: none single double single

    =========== =========== ===========
    Width 50%   Width 33%   Width 16%
    =========== =========== ===========
    Line 1      This text   This text
                should wrap will always
                onto        be one line.
                multiple
                lines.
    Line 2      Centered.   Right-Aligned.
    Line 3      Centered    Right-Aligned
                Again.      Again.
    =========== =========== ===========
    
    


.. DANGER::
   Beware killer rabbits!


.. table:: **Names/Ages**

    ==================   ============
    Name                 Age
    ==================   ============
    John D Hunter        40
    Cast of Thousands    41
    And Still More       42
    ==================   ============


.. tabularcolumns:: |p{0.80\linewidth}c|p{0.10\linewidth}l|


.. list-table::
   :header-rows: 1
   :widths: 5 10

   * - A1
     - B1

   * - da1
     - db1
     
   * - da2
     - db2
     
   * - da3
     - db3
     
   * - da4
     - db4
     
.. tabularcolumns:: |l|c|p{5cm}|
    
.. table:: This is my table

    +-------------+-----------------+-----------------------+
    |Field type   |Description      |Example                |
    +=============+=================+=======================+
    |Field type   |Description      |Example                |
    +-------------+-----------------+-----------------------+

