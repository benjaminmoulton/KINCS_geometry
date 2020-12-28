# KINCSgeometry
Creates the KINCS geometry

## Required Packages
The following packages are required to use run this code: airfoil_db, numpy, json, scipy, and shapely. The airfoil_db package can be found [here](https://github.com/usuaero/AirfoilDatabase).

## Running the Code
The code can be run by importing the code and running the main function with a single input of the input formatted JSON file location.

## Input File Format
The input file should have a tuple with the key "run". This tuple should have as indices string names. Each of these string names should reference a dictionary in the file. The following keys and corresponding types should be in that dictionary (For example, "run" : ["C18"]  , with the following in the "C18" dictionary).

>**"airfoil dat file location" : string**
>>The airfoil dat file to be used for the ARCS geometry. It must be in Selig format, with no text before the airfoil data.
>>
>**"chord [in]" : float**
>>The airfoil chord length in inches.
>>
>**"num kinks" : integer**
>>The number of kinks desired in the geometry. must be greater than or equal to 1.
>>
>**"kinkiness forward and aft" : tuple**
>>The percentage of the kink to be placed forward and aft of each connection point. For example: the tuple [0.5, 0.5] would result in a kink where half is before the connection, and half aft of the connection; the tuple [0.0, 1.0] would result in a kink where it is fully aft of the connection.
>>
>**"first kink forward" : boolean**
>>Whether the kink should curve toward the leading edge first.
>>
>**"kink degree" : integer**
>>The number of curves each kink should have. Should be greater than 1.
>>
>**"kinks to remove" : tuple**
>>The kinks to be removed for plotting / dxf output. Should be integer values between 1 and the number of kinks, inclusive.
>>
>**"fillet kinks" : boolean**
>>Whether to fillet the kink connections to the upper and lower surfaces of the airfoil. The structural strength is improved when this value is true.
>>
>**"fillet kinks sharps" : boolean**
>>Whether to fillet the sharp points on the kink curves. No significant difference is shown using this feature, and thus can be left as false.
>>
>**"report hinge point" : boolean**
>>Whether to print the hinge point to the screen. If true, reports the hinge point as """Hinge Point at (x/c,y/c) : ( xxxx , xxxx )""".
>>
>**"num kink points" : integer**
>>The number of points to create for each kink. Higher values are preferable for better resolution, 60 points works well.
>>
>**"kink shift [x/c]" : float**
>>The percent chord value to shift the kink aft. No significant improvement on compliance was noticed using this feater, and can thus be left as 0.0.
>>
>**"kink thickness type" : string**
>>Which type of kink thickness to create. The options and corresponding descriptions are:
>>>*"fixed"*
>>>>Create all kinks with the same thickness. requires that **"kink thickness [mm]"** be a tuple with a single value (for example, [0.5]).
>>>>
>>>*"gradient"*
>>>>Create all kinks with thicknesses based on a linear gradient between two values. requires that **"kink thickness [mm]"** be a tuple with a two values (for example, [0.5,0.45], where a kink at the hinge point will have thickness 0.5 mm, and a kink at the TE wall will have thickness 0.45 mm).
>>>>
>>>*"specific"*
>>>>Create all kinks with specific thicknesses. requires that **"kink thickness [mm]"** be a tuple with a value for each kink (for example, [0.5,0.45,0.35] for a KINCS mechanism with **"num kinks"** = 3).
>>>>
>**"kink thickness [mm]" : tuple**
>>The thickness for the kinks in millimeters as determined from the thickness type described above.
>>
>**"section resolution" : integer**
>>The section resolution of the airfoil. This value does not depend on the input airfoil file. A value above 200 is preferable.
>>
>**"shell thickness [mm]" : float**
>>The KINCS geometry skin thickness. A value of 0.8 mm works best for most morphing mechanism applications.
>>
>**"flex start [x/c]" : float**
>>The hinge point in percent chord.
>>
>**"tongue start [in]" : float**
>>The distance ahead of the hinge point where the tongue should begin, in inches. A value of 1 to 2 inches works best for most morphing mechanism applications with a 12 inch chord.
>>
>**"mouth start [in]" : float**
>>The distance ahead of the hinge point where the mouth should begin, in inches. A value of 2 to 3 inches works best for most morphing mechanism applications with a 12 inch chord. This value should always be greater than the "tongue start [in]" value.
>>
>**"TE wall start [x/c]" : float**
>>The wall location in percent chord. Past this location, the parabolic flap will have minimal effect on deflection. Thus, a value closer to 1 is preferable. A value of 0.95 works best for most morphing mechanism applications.
>>
>**"mouth clearance [mm]" : float**
>>The thickness between the tongue and the mouth surfaces. A value of 1.0 mm works best for most morphing mechanism applications with a skin thickness of 0.8 mm.
>>
>**"fillet side length [mm]" : float**
>>The filled side length to be used connecting the hinge point wall to the upper surface. A value of 1.0 mm works best for most morphing mechanism applications with a skin thickness of 0.8 mm.
>>
>**"enthicken" : float**
>>The thickness multiplier to modify the aspect of the airfoil. A value greater than 1 will result in a thicker airfoil, a value less than 1 will result in a thinner airfoil.
>>
>**"skip wing box" : boolean**
>>Whether to skip the wing box geometry during system generation.
>>
>**"I beam" : dictionary**
>>A dictionary of the values used to create the I beam structure. The keys of this dictionary are:
>>>**"thickness [mm]" : float**
>>>>The thickness of the I beam in millimeters. A value of 2.4 works well with a chord of 12 inches.
>>>>
>>>**"flange width [x/c]" : float**
>>>>The width of the flanges in percent chord. A value of 0.05 works best for morphing mechanism applications.
>>>>
>>>**"hollow [mm]" : float**
>>>>The hollow thickness within the I beam. Depending on the I beam **"thickness [mm]"**, a value of 0.8 works best for morphing mechanism applications.
>>>>
>>>**"make hollow" : boolean**
>>>>Whether to make the I beam hollow. Making the I beam hollow decreases the weight and customizes the infill of the I beam.
>>>>
>**"leading edge thickness[mm]" : float**
>>The KINCS geometry skin thickness at the leading edge in the wing box. A value of 0.8 mm works best for most morphing mechanism applications.
>>
>**"show plot" : boolean**
>>Whether to show a plot of the morphing airfoil.
>>
>**"write dxf" : boolean**
>>Whether to write the airfoil shape to a DXF file.
>>
>**"shift to c/4" : boolean**
>>Whether to shift the airfoil so the quarter chord is at the origin.
>>
>**"split return" : boolean**
>>Whether to split off the outer shape for use in generating the files used to model the Horizon aircraft.
>>
>**"guide curve return" : boolean**
>>Whether to split off geometric values concerning the guide curves for use in generating the files used to model the Horizon aircraft.
>>
>**"actuation hole return" : boolean**
>>Whether to split off geometric values concerning the actuation location for use in generating the files used to model the Horizon aircraft.
>>
>**"show legend" : boolean**
>>Whether to show the legend of the parts of the ARCS geometry when plotting.
>>
>**"all black" : boolean**
>>Whether to plot the geometry all in black.
>>
>**"deflection angle [deg]" : float or list**
>>Which deflection angles to examine on the plot. If float given, no deflection will be shown. If a list is given, all deflection values in the list will be plotted on the figure.
>>
>**"deflection type" : string**
>>Which type of deflection to create. The options and corresponding descriptions are:
>>>*"none"*
>>>>Plots no deflection.
>>>>
>>>*"traditional"*
>>>>Plots articulated deflection(s) at the amount(s) specified.
>>>>
>>>*"parabolic"*
>>>>Plots parabolic deflection(s) at the amount(s) specified.
>>>>
>**"hinge at top" : boolean**
>>Whether to locate the hinge point in the middle of the skin on the upper surface. If false, the y-value of the hinge point remains at 0.
>>
>**"dxf file path" : string**
>>The file path where to export the morphing airfoil DXF file (example : "/home/user/Desktop/").
>>
>**"dxf file name" : string**
>>The file name of the exported DXF file (example : "NACA_2412_KINCS").
>>
