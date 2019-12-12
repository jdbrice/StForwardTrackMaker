### StForwardTracker Development 

See `docker/README.md` for direction for setting up and building the docker images.

Develop inside the container:
```
docker run --rm --name FWD -ti -w /tmp/work -v <path_to_repo>/StRoot:/tmp/star-sw/StRoot -v <path_to_repo>/work:/tmp/work star-fwd bash
```
this will start you inside the container with you local `StRoot` and `work` directories from this repo mounted.
You can edit the code/files in these directories and run them inside the container.
NOTE: the `--rm` flag means that the container will be destroyed when you run `exit` from inside the container. Your work should be saved in the mounted volumes, so this should be OK, if not remove the `--rm` flag.

Inside the container your working directory should be `/tmp/work/`

## setup STAR environment
```sh
source star_env
```
this sets up the STAR environment variables and allows you to use `root4star`, `starsim`, etc.

## Build changes to your code
```sh
./build
```
NOTE: when you first start the container you may need to:
```sh
touch /tmp/star-sw/StRoot/StgMaker/StgMaker.cxx
./build
```
to make sure it builds the code - sometimes it does not pickup changes when the container is first started.

## Run the forward tracking code
```sh
root4star -b -q -l simple.C
```

## Edit the configuration 
The configuration file used when you run the above command is `config.xml`
You can modify this file to change the configuration of the ForwardTrack maker.

Example configuration and some information about the various parts below:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<config>

	<!-- The below line is still needed, do not change for now -->
	<Input url="SimHitsTuple_1track_0p2_5.root" type="sim_mc" max-events="200" first-event="0" si="false" />

	<!-- This is the output file for the QA ROOT file produced by the tracking code -->
	<Output url="results.root" />

	<!-- CONFIGURATION for Track finding -->
	<TrackFinder nIterations="1">
		<Iteration> <!-- Options for first iteration -->
			<SegmentBuilder>
				<Criteria name="Crit2_RZRatio" min="0.9" max="1.11" />
				<Criteria name="Crit2_DeltaPhi" min="0" max="10" />	
				<Criteria name="Crit2_DeltaRho" min="-5" max="20"/>
				<Criteria name="Crit2_StraightTrackRatio" min="0.9" max="1.1"/>
			</SegmentBuilder>

			<ThreeHitSegments>
				<Criteria name="Crit3_3DAngle" min="0" max="90" />
				<Criteria name="Crit3_PT" min="0" max="100" />
				<Criteria name="Crit3_ChangeRZRatio" min="0" max="1.01" />
				<Criteria name="Crit3_2DAngle" min="0" max="2" />
			</ThreeHitSegments>
		</Iteration>

		<!-- These are used if not defined inside <Iteration> -->
		<ThreeHitSegments>
			<Criteria name="Crit3_2DAngle" min="0" max="50" />
		</ThreeHitSegments>

		<Connector distance="1"/>

		<SubsetNN active="true" min-hits-on-track="4" >
			<Omega>0.99</Omega>
			<StableThreshold>0.001</StableThreshold>
		</SubsetNN>	

		<HitRemover active="true">
		</HitRemover>

	</TrackFinder>

	<!-- CONFIGURATION for Track Fitting -->
	<TrackFitter constB="true" display="false" noMaterialEffects="true" >
		<Vertex sigmaXY="0.01" sigmaZ="5" includeInFit="true" />
		
		<!-- for MC only -->
		<Hits sigmaXY="0.01" useFCM="true" />
	</TrackFitter>


</config>

```
