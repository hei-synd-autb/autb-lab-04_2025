# Modification pour Lab 04.

Il y a un peu plus de boulot que pour les autres.

## Enum
Premièrement, les Enum ``E_ExecuteDone`` et ``E_InOperationBase`` existent déjà dans le Package HEVS_Pack_2022.

## PackML

Remplacer ce genre de truc avec les état / mode PackML
// fbGripperState.Enable := (stPlcOpenFbs.bEnableRemote)        		OR
//                    		 (NOT stPlcOpenFbs.bEnableRemote 		AND 
// 				   		  NOT (fbPackStates.state.Aborting OR
//                                fbPackStates.state.Aborted));

fbGripperState.Enable := ((PackTag.Status.UnitModeCurrent = E_PackModes.Manual))        		OR
                   		 (NOT (PackTag.Status.UnitModeCurrent = E_PackModes.Manual) 		AND 
				   		  NOT (PackTag.Status.StateCurrent = E_PackState.eAborting) OR
                              (PackTag.Status.StateCurrent = E_PackState.eAborted));	

## Robot
Il faut créer une séquence automatique du robot avec uniquement l'axe X. pour faire un aller et retour.

C'est pour cela que les lignes suivantes ne permettent pas la compilation.

stStateMachineInfo: ST_StateMachineInfo;

fbOpenGripper.Execute := (stStateMachineInfo.eState = E_Execute.eMotionBackDone) OR (eStarting = E_Starting.eMotionStartingDone);
fbCloseGripper.Execute := (stStateMachineInfo.eState = E_Execute.eMotionFwdDone);

Ce programme doit être écrit dans ACT_OneUnitPick();
Il faut prendre garde à séparer fortement 
-   ACT_OneUnitPick();
-   ACT_ManualJog();

Pour travailler une fois en mode manuel et un fois en mode automatique.


## User Interface.
Il faut partir de ce machin: Move_Axes_flows.json dans Move_Axes_flows.json
Supprimer le répartoir HMI qui est laissé là uniquement pour savoir à quoi il ressemblait.



## Autre idée: utiliser le FB du labo 03 pour détecter une position de l'axe et ouvrir le gripper quand il est à une certaine distance du capteur.


