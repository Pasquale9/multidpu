# Multidpu
In questa repository sono presenti il driver e il componente della libreria Vitis-AI modificati per consentire il funzionamento di architetture con più DPU instanziate su fpga Xilinx in parallelo.
In particolare il driver consente di fare ciò creando 2 dispositivi a caratteri (uno per ogni DPU) chiamati `dev/multidpu0` e `dev/multidpu1`. 
Mentre la libreria è stata progettata in modo da poter selezionare la DPU da utilizzare mediante una variabile d'ambiente chiamata ``DPU_DEVICE_NAME``.
Per esempio se volessimo usare la prima DPU dovremmo settare:
```bash
DPU_DEVICE_NAME="multidpu0".
```

# Installazione
Per far funzionare il tutto bisogna aggiungere il driver come modulo kernel, come specificato readme presente nei sorgenti. 
Per la libreria VITIS-AI invece bisogna sostituire la cartella vart, qui presente, con quella che si trova nella libreria al percorso: "Vitis-AI-3.0/src/vai_runtime", sostituire il file xdputil_query.cpp con quello presente nella libreria al percorso: "Vitis-AI-3.0/src/vai_library/usefultools/src/" e ricompilare il modulo. 

## Come usare `recipes-vitis-ai`
0. è possibile recuperare tutti i sorgenti da github al link: https://github.com/Xilinx/Vitis-AI.git

1. Copia la cartella `recipes-vitis-ai` in `<progetto petalinux>/project-spec/meta-user/`:
```bash
cp src/vai_petalinux_recipes/recipes-vitis-ai <progetto petalinux>/project-spec/meta-user/
```
 
2. Modifica il file `<progetto petalinux>/project-spec/meta-user/conf/user-rootfsconfig`, aggiungendo le seguenti righe:
```
CONFIG_vitis-ai-library
CONFIG_vitis-ai-library-dev
CONFIG_vitis-ai-library-dbg
CONFIG_vai-benchmark
CONFIG_vai-sample
```
 
3. Esegui il sourcing dello strumento PetaLinux e poi esegui il comando `petalinux-config -c rootfs`. Seleziona la seguente opzione:
```
Seleziona pacchetti utente --->
Seleziona [*] vitis-ai-library
```
Salva e esci.
#### Nota:
Per personalizzare la libreria è necessario apportare le modifiche desiderate su github (facendo una fork del repository originale) e aggiornare il file "recepies-Vitis-ai/vitis-ai-library/vitisai.inc" inserendo il proprio link git e facendo attenzione ad inserire nella variabile SRCREV il corretto hash del commit che si vuole scaricare.


4. Esegui `petalinux-build`.

### Troubleshooting 
Si sono riscontrati problemi durante la build del modulo, in particolare durante la fase del "do_fetch" di Unilog che abbiamo risolto allinenado i commit inserendo il giusto codice hash nel file vitisai.inc, inoltre abbiamo notato che bisognerebbe aspettare alcune ore per fare in modo che le modifiche si propaghino. E consigliamo di ripulire la cache del progetto tra le build tramite i comandi:
```
petalinux-build -x mrproper
petalinux-build -x cleansstate
```
