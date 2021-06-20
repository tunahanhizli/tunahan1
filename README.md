# tunahan1
wipe

model basic -ndm 2 -ndf 2

node 1 0.0 0.0
node 2 3.0 0.0
node 3 6.0 0.0
node 4 0.0 4.0
node 5 3.0 4.0
node 6 6.0 4.0


fix 1 1 1
fix 2 0 1
fix 3 1 1

# uniaxialMaterial Steel01 $matTag $Fy $E0 $b
# Fy = 420 MPa ve E=200 GPa
uniaxialMaterial Steel01 100 420.0e+6 200.0e+9 0.01


# element truss $eleTag $iNode $jNode $A $matTag , A = 0.05 m^2
element truss 1 1 2 0.05 100
element truss 2 2 3 0.05 100
element truss 3 1 4 0.05 100
element truss 4 1 5 0.05 100
element truss 5 4 2 0.05 100
element truss 6 2 5 0.05 100
element truss 7 2 6 0.05 100
element truss 8 5 3 0.05 100
element truss 9 3 6 0.05 100
element truss 10 4 5 0.05 100
element truss 11 5 6 0.05 100

# Lineer zaman serisi
timeSeries Linear 200

pattern Plain 1 200 {

load 5 0.0 -100000.0
load 6 50000.0 0.0
}

# Create the system of equation
system BandSPD
    
# Create the DOF numberer, the reverse Cuthill-McKee algorithm
numberer RCM
    
# Create the constraint handler, a Plain handler is used as homo constraints
constraints Plain

# Create the integration scheme, the LoadControl scheme using steps of 1.0
integrator LoadControl 0.1

# Create the solution algorithm, a Linear algorithm is created
algorithm Linear

# create the analysis object 
analysis Static 

analyze 10

print -node 3
print -ele 6
