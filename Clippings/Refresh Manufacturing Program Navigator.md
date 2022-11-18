# Refresh Manufacturing Program Navigator
Updating the graphics display using UF\_DISP\_refresh will not work for any navigator.

Also how the DLL is loaded will not make any difference too.

The operation navigator has multiple views, so you would do something like the following:

`

1.  Dim theUFSession As  UFSession  =  UFSession.GetUFSession()

3.  Dim theCurrentOntView As UF.UFUiOnt.TreeMode  =  Nothing

5.  theUFSession.UiOnt.AskView(theCurrentOntView)
6.  theUFSession.UiOnt.SwitchView(UF.UFUiOnt.TreeMode.MachineMode)
7.  theUFSession.UiOnt.Refresh()
8.  theUFSession.UiOnt.SwitchView(UF.UFUiOnt.TreeMode.GeometryMode)
9.  theUFSession.UiOnt.Refresh()
10.  theUFSession.UiOnt.SwitchView(UF.UFUiOnt.TreeMode.MachineTool)
11.  theUFSession.UiOnt.Refresh()
12.  theUFSession.UiOnt.SwitchView(UF.UFUiOnt.TreeMode.Order)
13.  theUFSession.UiOnt.Refresh()
14.  theUFSession.UiOnt.SwitchView(theCurrentOntView)

`

This makes sure that all four operation navigator views are up-to-date.