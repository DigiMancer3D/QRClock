# QRClock
Algorithmically Controlled Data Clock with QR Output Capabilities
</br></br>
## QRs can be so specific that people could use them as a clock
<hr>
</br>
That's the main perspective that started this build but since then the clock has becaome more about determining very specfic points of data simply off the UTC of the viewer.
</br></br>
There's tons of points of data displayed directly on the clock face allwoing the user to determine as close as the 1/8th hour without knowing the exact time amoung other things.
<hr>
</br></br>
<h1>GameClock</h1>
Algorithmically Faked-Data Clock 
</br>
<h2>Why not have a seemingly real clock for game timing?</br>
<h3>This still runs off the principle that there are ways to have a clock face without saying the time and still display tons of information without text/words.</h3></h2>
</br></br>
&nbsp; &nbsp; &nbsp; This clock has a handful of algos that handle various concepts for websites, webapps and games alike to get that day-time presense. The algos within handle the following series of events:
</br></br>
<ul><ol> -  Various Sun/Moon Data <ul>Sunrise & Sunset (times & perceived [both phyical & digital-global background])</ul><ul>Adjusting Sunrise/Sunset for global bi-yearly time adjustments</ul><ul>Sun:Moon Superposition (Eclipse Guesser)</ul></ol></br><ol> -  Various Epoch based data </br><ul>Epoch Lag (Epoch:Now Superpositioning)</ul><ul>Epoch Length (full)</ul><ul>Epoch Time Slices (sections of the epoch [including multiples])</ul><ul>Epoch Year</ul></ol></br><ol> -  Various Phyiscal Data </br><ul>Year</ul><ul>Month <ol> - Days in the current month</ol></ul><ul>Day</ul><ul>Hour <ol> - Quater-Hour position</ol><ol> - Eigth-Hour position</ol></ul><ul>Minutes</ul><ul>Seconds</ul><ul>Millaseconds</ul><ul>Day-Merdian position</ul><ul>Approximate Shadow Realitive positioning</ul><ul>Continued Perceived Time</ul><ul>[Perceived] Quantum Gradients<ol> - External via Internal Perspective</ol><ol> - Internal via Internal Perspective</ol><ol> - Individal via [limited] External Perspective </ol></ul><ul>Approximate Global Average Temprature <ol> - Color representation of tempurature</ol><ol> - Percentage of accuracy/coverage from current location</ol><ol> - The number the temprature is calculated from</ol></ul></ol></br><ol> -  Various Comparable Data </br><ul>Month:Year Superposition</ul><ul>Year Natural Superpositioning (used for comparing other superpositions)</ul><ul>Year:You Superposition</ul><ul>Year:Epoch Year Superposition</ul><ul>Base64 Output of the Epoch</ul><ul>Day Progression Percentage</ul><ul>Hour Progression Percentage</ul><ul>Minute Progression Percentage</ul></ol></br><ol> -  Various Manipulated-Generated Data </br><ul>Yearums (hex-style hash of year based data-manipulated outputs)</ul><ul>Yearum Lengths (actual data)</ul><ul>Some of the Epoch Time Slices</ul><ul>Reversed Time (negitive version) <ol> - Rversed seconds</ol><ol> - Reversed Minutes</ol><ol> - Reversed Hours</ol><ol> - Reversed Quater & Sixth Hour positions</ol></ul><ul>Not-Time (the "not current time" offset by 100) <ol> - Not-Today</ol><ol> - 2cd Greade Readable Time</ol><ol> - 2cd Grade Reversed Time (Not-Time [fully offset "Not the current time" by 100])</ol></ul><ul>3rd Rite Time Percentage</ul><ul>Various Galactic hours (array)</ul><ul>Possible Time-Based Weather Color-Data (array of Hash-based colors [#123aef])</ul></ol></ul>
</br><hr></br></br>

<h2>Faking the Epoch</h2>
<hr></br>
&nbsp; &nbsp; &nbsp;The epoch (time since Midnight Jan 1 1970) is what many web-based systems use to keep track of millasecond or slower timescales. The benefits of faking the epoch is being able to fully convience a system that it is of a different time. So we already know we need a scheme to build an epoch then to update that epoch overtime without re-writing the old epoch. We have a set place in the webapp HTML for an epoch storage, so we will use this to pass the epoch between various functions. We also can use this as a method of determining "first run" to build the fake epoch. Let's start faking numbers.</br>
&nbsp; &nbsp; &nbsp;We are currently in a strange year point of the epoch. The furthest point of the epoch (digit furthest to the left) is currently a "1" & many people alive today may see the day it changes to a "2" in 2033. So, we are set at the moment to no even randomly pick a number for this spot. We just pick "1" and later we will set a roll-over that would allow this digit to increase overtime, then in 2033 we'll need to update this to have range but direct that pointer still closer to the 2 then the 1. The second furthest point as of writing is a "7" but may soon be an "8", it just picks "7" for now but it should randomly pick a number between 5 & 9 with a modifier to add an additional "1" after the pick to allow overflow-rollover. Then adjust if the number is below 7 then rollover again if 9 is overflowed. We will see examples of this direct and adjust after picking methods later but for now after this point all the other 11 points will be randomly picked numbers between 0 & 9 then modify after the pick to add an additional "1" to allow overflow and rescope above the 0 for a chance of 1 - 10.</br></br>
&nbsp; &nbsp; &nbsp;Allowing 9 to overflow (having a result past the number 9) will allow a base-scale of 10 to overflow, in-example: when you add 1 to 9 (1+9), you overflow to the tens spot by ending with "10" where 1 is in the tens (so one-tens) and the 0 is in the ones (so zero-ones) which does describe the number ten. We want to do all the number picking at the same time then immedieately rollover all 9-overflows starting with the closest point. This means if any digit overflows, it will affect the next to check for overflow in-other-words, we are overlfowing to the left and starting furthest to the right so we always cover our tracks. You do not need to check for overflow on the furthest away (last digit on the left) because it will naturally overflow in the same direction & we only need to adjust this as we climb up in the epoch in our-time-scale.</br>
&nbsp; &nbsp; &nbsp;At the moment we are generating an epoch without any comment of the real-world around the algo. You could easily check real epoch length and the first 3 digits (furthest 3 digits on the left side) to get all the info you need to fake an epoch near to reality or even perfectly offset to seem like you are phyiscally in a different location (<s>Why Mask an IP When You Can Mock a New Location for a 100% real IP for a location that may not even exist</s> [except for UTC] <i>;} ;}</i> ).
</br></br></br>

<h2>Faking UTC TimeZones</h2>
<hr></br>
&nbsp; &nbsp; &nbsp;We are faking the TimeZone because we've used the TimeZone online web-help to obtain various time-scale jumps & changes to help adjust for Daylight-Savings & other anomlies in global time-scaling. We do still have issues where some areas will jump thier exact adjust change for daylight-savings but in the wrong direction. So some locations will see the screen get dark an hour early or begin to lighten up an hour early.






