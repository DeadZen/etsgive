# Demonstration of [Don't lose your ETS Tables](http://steve.vinoski.net/blog/2011/03/23/dont-lose-your-ets-tables/)

----
## Let It Crash...Except When You Shouldn't

> When you create an ets table you can also name a process to inherit the table should the creating process die

----
### Example Session

> rebar co && erl -pz ebin -s etsgive

    Eshell V5.9.2  (abort with ^G)
    1> MGR(<0.38.0>) -> SRV(<0.37.0>) getting TableId: 16400
    etsgive_srv:count().
    ok
    Counter: 1
    2> etsgive_srv:count().
    ok
    2> etsgive_srv:count().
    Counter: 2
    ok
    3> etsgive_srv:count().
    Counter: 3
    ok
    4> etsgive_srv:count().
    Counter: 4
    ok
    5> etsgive_srv:count().
    Counter: 5
    ok
    6> exit(whereis(etsgive_srv), kill).  
    true
    7> Warning TableId: 16400 OwnerPid: <0.37.0> is dying
    SRV(<0.37.0>) => MGR(<0.38.0>) handing TableId: 16400
    SRV(<0.37.0>) !! is now dead, farewell TableId: 16400
    MGR(<0.38.0>) -> SRV(<0.44.0>) getting TableId: 16400
    7> etsgive_srv:count().             
    ok
    Counter: 6
    8> etsgive_srv:count().
    ok
    Counter: 7
    9> etsgive_srv:die().               
    ok
    10> Warning TableId: 16400 OwnerPid: <0.44.0> is dying
    SRV(<0.44.0>) => MGR(<0.38.0>) handing TableId: 16400

    =ERROR REPORT==== 24-Apr-2013::11:01:48 ===
    ** Generic server etsgive_srv terminating 
    ** Last message in was {'$gen_cast',die}
    ** When Server state == {state,true,16400}
    ** Reason for termination == 
    ** killed
    SRV(<0.44.0>) !! is now dead, farewell TableId: 16400
    MGR(<0.38.0>) -> SRV(<0.48.0>) getting TableId: 16400
    10> etsgive_srv:count().
    Counter: 8
    ok
    11> [ exit(whereis(etsgive_srv), kill) || _ <- lists:seq(1, 20) ].
    [true,true,true,true,true,true,true,true,true,true,true,
    true,true,true,true,true,true,true,true,true]
    12> Warning TableId: 16400 OwnerPid: <0.48.0> is dying
    SRV(<0.48.0>) => MGR(<0.38.0>) handing TableId: 16400
    SRV(<0.48.0>) !! is now dead, farewell TableId: 16400
    MGR(<0.38.0>) -> SRV(<0.51.0>) getting TableId: 16400
    12> etsgive_srv:count().                                          
    ok
    13> Counter: 9
