Homework 3:

Task 1: 
For task 1 we have keep the first position of the agent as the position of the base, and the first position of 
the objective as the position of the flag, both at the method init. Then at the plan !perform_look_action that is 
always executed, the agent will print its position, then using the function distance calculates the distances and
prints the results.

+!perform_look_action
   <- ?debug(Mode); if (Mode<=1) {  

      .println("");

    } 

    //calculo de mi posicion
    ?my_position(X, Y, Z);
    .println("ESTOY EN LA POSICION: ", X," ; ", Y," ; ",Z);

    //calculo de la distancia a la bandera
    ?flag(FlagX, FlagY, FlagZ);
    !distance( pos(X, Y, Z), pos(FlagX, FlagY, FlagZ));
    ?distance(D);
    .println("DISTANCIA A LA BANDERA: ", D);

    //calculo de la distancia hasta la base
    ?base(BX, BY, BZ);
    !distance( pos(X, Y, Z), pos(BX, BY, BZ));
    ?distance(D2);
    .println("DISTANCIA A LA BASE: ", D2);.

+!init
   <- ?debug(Mode); if (Mode<=1) { .println("YOUR CODE FOR init GOES HERE.")}
   ?my_position(X, Y, Z);
    +base(X, Y, Z);
    ?objective_position(FlagX, FlagY, FlagZ);
    +flag(FlagX,FlagY,FlagZ);.


Task2:
For task 2 we have augmented the radious of patrolling. This makes the agent goes to random position around all the map.
The random positions are checked by the method patrolling that is include in jgomas.asl

patrollingRadius(300).

Task3:
For task 3, at the plan !perform_look_action of the crazzy agent, crazzy agent send his position to the other axis agent,
and the other axis agent go to the position that cazzy agent tells.

Crazzy agent:
+!perform_look_action 
  <- ?debug(Mode); if (Mode<=1) { .println("YOUR CODE FOR PERFORM_LOOK_ACTION GOES HERE.") }

    ?my_position(X, Y, Z);
    .my_team("medic_AXIS",E);
    .nth(0, E, AgE);
    .concat("goto(",X,", ", Y, ", ", Z, ")", Content1);
    .send_msg_with_conversation_id(AgE, tell, Content1, "INT");


Other agent:
+goto(Xag,Yag,Zag)[source(A)] 
  <-
  .println("Recibido mensaje goto de ", A);
  !add_task(task("TASK_GOTO_POSITION",A,pos(Xag,Yag,Zag),""));
  -+state(standing);
  -goto(_,_,_).

Task4:
For task 4 crazzy agent send his position to the allied agent and the allied agent go to the position that crazzy agent
tells. When any agent is aimed by the allied agent, the allied agent start shooting the aimed agent. Our crazzy agent 
can't defend himself.

Crazzy agent:
+!perform_look_action 
  <- ?debug(Mode); if (Mode<=1) { .println("YOUR CODE FOR PERFORM_LOOK_ACTION GOES HERE.") }

    ?my_position(X, Y, Z);
    .my_team("medic_AXIS",E);
    .nth(0, E, AgE);
    .concat("goto(",X,", ", Y, ", ", Z, ")", Content1);
    .send_msg_with_conversation_id(AgE, tell, Content1, "INT");

    .my_team("ALLIED",E1);
    .nth(0, E1, AgA);
    .concat("goto(",X,", ", Y, ", ", Z, ")", Content2);
    .send_msg_with_conversation_id(AgA, tell, Content2, "INT");

Allied agent:
+goto(Xag,Yag,Zag)[source(A)] 
  <-
  .println("Recibido mensaje goto de ", A);
  !add_task(task("TASK_GOTO_POSITION",A,pos(Xag,Yag,Zag),""));
  -+state(standing);
  -goto(_,_,_).


Task 5:
For task 5, the crazy agent send to other agent his position adding a radious. The other agent start patrolling around 
the crazy agent. 

+!perform_look_action 
  <- ?debug(Mode); if (Mode<=1) { .println("YOUR CODE FOR PERFORM_LOOK_ACTION GOES HERE.") }

    ?my_position(X, Y, Z);
    .my_team("medic_AXIS",E);
    .nth(0, E, AgE);
    .concat("goto(",X,", ", Y, ", ", Z, ")", Content1);
    .send_msg_with_conversation_id(AgE, tell, Content1, "INT");

    .random(A);
    .random(B);

    Anew= 50/2 - A*50;
    Bnew= 50/2 - B*50;

    Xnew= X+ Anew;
    Znew= Z+ Bnew;

    .nth(1,E, AgE2);
    .concat("goto(",Xnew,", ", Y, ", ", Znew, ")", Content3);
    .send_msg_with_conversation_id(AgE2, tell, Content3, "INT");