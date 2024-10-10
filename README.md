# Multidpu
In questa repository sono presenti il driver e il componente della libreria Vitis-AI modificati per consentire il funzionamento di architetture con più DPU instanziate su fpga Xilinx in parallelo.
In particolare il driver consente di fare ciò creando 2 dispositivi a caratteri (uno per ogni DPU) chiamati dev/multidpu0 e dev/multidpu1. 
Mentre la libreria è stata progettata in modo da poter selezionare la DPU da utilizzare mediante una variabile d'ambiente chiamata DPU_DEVICE_NAME.
Per esempio se volessimo usare la prima DPU dovremmo settare DPU_DEVICE_NAME="multidpu0".

# Installazione
Per far funzionare il tutto bisogna aggiungere il driver come modulo kernel, come specificato readme presente nei sorgenti. 
Per la libreria VITIS-AI invece bisogna sostituire la cartella vart, qui presente, con quella che si trova nella libreria al percorso: "Vitis-AI-3.0/src/vai_runtime" e ricompilare il modulo. 

