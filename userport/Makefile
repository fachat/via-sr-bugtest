
all: mpsndt mpsndp mprcv mvsnd mvrcv


mpsndp: mpsnd.a65
	xa -DPHI2 -o $@ $< 

mpsndt: mpsnd.a65
	xa -DT2 -o $@ $< 

mprcv: mprcv.a65
	xa -o $@ $< 

mvsnd: mvsnd.a65
	xa -o $@ $< 

mvrcv: mvrcv.a65
	xa -o $@ $< 

