#!/bin/bash 

source /usr/local/gromacs/bin/GMXRC

j=0
frames_list='0 831 890 920 966 998 1010 1049 1056 1077 1090 1099 1136 1154 1224 1245 1276 1327 1343 1375 1403 1413 1452 1585 1632 1655 1706 1731 1838 1870 1876 1916 1932 2055 2059 2111 2156 2186 2200 2229 2350 2365 2427 2483'
for i in frames_list; do
     let 'j=j+1'
     gmx grompp -f md_umbrella.mdp -c ./recalc/gtp_perl/distg${i}.gro -p ./UNK_gtp_pull/topol_UNK_gtp.top -n ./UNK_gtp_pull/UNK_gtp2.ndx -o ./recalc/gtp_perl/umbrellG3_${j}.tpr -maxwarn 999
     cp ./recalc/r ./recalc/gtp_perl/r
     mv ./recalc/gtp_perl/r ./recalc/gtp_perl/rG3_${j}

     #adding script-specific lines
     echo "#SBATCH -o G3_${j}_o.out" >> ./your/dir/gtp_perl/rG3_${j} 
     echo "#SBATCH -e G3_${j}_e.out" >> ./your/dir/gtp_perl/rG3_${j}
     echo "#SBATCH -J G3_${j}" >> ./your/dir/gtp_perl/rG3_${j}
     echo "#SBATCH --export=ALL" >> ./your/dir/gtp_perl/rG3_${j}
     echo "module unload intel" >> ./your/dir/gtp_perl/rG3_${j}
     echo "module load gromacs" >> ./your/dir/gtp_perl/rG3_${j}
     echo 'exe=`which gmx_mpi`' >> ./your/dir/gtp_perl/rG3_${j}
     echo 'processors=$(( $SLURM_NNODES * $SLURM_NTASKS_PER_NODE ))' >> ./your/dir/gtp_perl/rG3_${j}
     echo -n 'mpirun  -np $processors -genv I_MPI_FABRICS shm:ofa $exe mdrun -v -deffnm umbrellG3_' >> ./your/dir/gtp_perl/rG3_${j}
     echo "${j} -cpo umbrellG3_${j}" >> ./your/dir/gtp_perl/rG3_${j}
     echo "~" >> ./your/dir/gtp_perl/rG3_${j}                  

done
