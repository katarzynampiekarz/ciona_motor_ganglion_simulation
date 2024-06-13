# _Ciona_ Motor Ganglion simulation v2

## About
_Ciona_ has a simple central nervous system that consists of 177 neurons and their connections have been mapped (Ryan et al., 2016). Moreover, it is a malleable experimental model, highly genetically tractable, with established CRISPR-based gene manipulation methods, which makes it a good organism to use in conjunction with computational modeling.

The initial aim of the simulation was to investigate the role of ddNs, midline-crossing cells believed to be homologous to Mauthner cells in fish. A simulation is a great way to explore _in silico_ how various MG architectures affect the function of the circuit. Here, MG circuit was based on the diagram taken from Ryan et al., (2017): 

<p align="center">
<img src="https://github.com/katarzynampiekarz/ciona_motor_ganglion_simulation/blob/main/MG_diagram.png?raw=true" width="400">
</p>

Then the connections between ddNL, ddNR and the remaining cells were inverted to make ddNs act ipsilaterally, instead of contralaterally (see “Altered ddN connectivity” section below). However, the lack of ground truth in the form of electrophysiological data makes it impossible to properly optimize each parameter, therefore a lot of assumptions had to be made. This may limit the use of the simulation, however it can be easily updated, once the pertinent experimental data is available.<br>

The simulation was coded in NetPyNE and included 38 uniquely defined cells, and 394 connections in total. The cell soma diameter and length were calculated from the soma volumes given in Ryan et al. (2016) and HH mechanisms were inserted. The x, y, z positions of the MG cells and the connections between them were specified according to Ryan et al. (2016). The location of the remaining cells, for which there were no coordinates published, was approximated based on the diagrams available in the literature. The synapses were defined as Exp2Syn and included the excitatory synapses, the inhibitory synapses, and gap junction approximation. A NetStim artificial spike generator was used to deliver input to ddNR.

## Cells
The core Motor Ganglion is composed of:

1)	motor neurons (MN) – 5 pairs, acetylcholine is the putative neurotransmitter
2)	MG interneurons (MGIN) – 3 pairs of ventral MG neurons with descending ipsilateral axons; acetylcholine
3)	ascending MG peripheral interneurons (AMG) – 7 dorsal cells (3 on the left, 3 on the right, one central); GABA, acetylcholine (all cells were defined as inhibitory, except for AMG5, which was defined as excitatory)
4)	descending decussating neurons (ddN) – the anteriormost pair of neurons with long axons that cross the midline; acetylcholine
   
Additionally, the following neurons were included in the network:

1)	ascending contralateral inhibitory neurons (ACIN) – 3 cells; glycine
2)	posterior MG interneurons (PMGN) – 2 descending ipsilateral cells; GABA
3)	midtail neurons (mt) – 4 cells; excitatory
4)	bipolar tail neurons (BTN) – 4 cells; BTNs 1-2 were assumed excitatory, while BTNs 3-4 were coded as inhibitory

## Connectivity
The cumulative depth of synaptic contact from Ryan et al. (2016) was used as a proxy for synaptic weights. 

Connection strength matrix for chemical synapses:
<p align="center">
<img src="https://github.com/katarzynampiekarz/ciona_motor_ganglion_simulation/blob/main/synapses_connectivity.png?raw=true" width="700">
</p>
Connection strength matrix for putative gap junctions:
<p align="center">
<img src="https://github.com/katarzynampiekarz/ciona_motor_ganglion_simulation/blob/main/gap_junctions_connectivity.png?raw=true" width="700">
</p>
The top-down view of the network and connections:
<p align="center">
<img src="https://github.com/katarzynampiekarz/ciona_motor_ganglion_simulation/blob/main/v2_plot2Dnet_top_down_view.png?raw=true" width="700">
</p>

## Altered ddN connectivity
In the first simulation the ddNs were connected contralaterally, as they are in the true, biological circuit. This connectivity pattern allows ddNs to act on the contralateral motor neurons, which may allow the larva to escape away from the side of the stimulation. This would be expected, given that the ddNs are believed to be homologous to the Mauthner cells present in the fish, which take part in the startle response (Ryan et al., 2017). Here, the stimulus applied to ddNR leads to the activation of the contralateral MNs (MN2L, MN3L, MN5L), which would result in the contraction of the left-side muscles. This should produce a unilateral tail flick, which turns the larva 90 deg, and would be consistent with the network activity described in Ryan et al. (2017).

 <p align="center">
<img src="https://github.com/katarzynampiekarz/ciona_motor_ganglion_simulation/blob/main/MG_sim_v2_raster.png?raw=true" width="700">
</p>

However, in the second simulation, where the connections of ddNR and ddNL were swapped, so the contralateral connections are absent, after the initial response from the left MNs, stimulating ddNR leads to the simultaneous spiking of the right MNs, as well as ddNL. This activity pattern probably would not allow for a proper re-orienting unilateral tail flick and a successful escape.

<p align="center">
<img src="https://github.com/katarzynampiekarz/ciona_motor_ganglion_simulation/blob/main/MG_sim_v2_ddNs_swapped_raster.png?raw=true" width="700">
</p>


## Caveats
1)	Lack of crucial electrophysiology data from _Ciona_’s neurons, therefore  <ins>__many__</ins> assumptions had to be made
2)	Currently there is no gap junction implementation in NetPyNE – the user needs to approximate
3)	Further optimization and refinement needed!

## References
Ryan, K; Lu, Z; Meinertzhagen, IA. (2016) The CNS connectome of a tadpole larva of Ciona intestinalis (L.) highlights sidedness in the brain of a chordate sibling eLife 5:e16962.

Ryan, K., Lu, Z., Meinertzhagen, I.A. (2017). Circuit homology between decussating pathways in the Ciona larval CNS and the startle-response pathway. Current Biology 27, 721-728.

