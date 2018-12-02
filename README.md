# DITRAS

DITRAS (DIary-based TRAjectory Simulator) is a framework to simulate the spatio-temporal patterns of human mobility in a realistic way. DITRAS operates in two steps: the generation of a *mobility diary* and the translation of the mobility diary into a *mobility trajectory*. The mobility diary is constructed by a Markov model which captures the tendency of individuals to follow or break their routine. The mobility trajectory is produced by a model based on the concept of preferential exploration and preferential return. DITRAS can reproduce the statistical properties of real human mobility trajectories in an accurate way. For technical details on DITRAS please read the scientific article at this link: [http://arxiv.org/abs/1607.05952](http://arxiv.org/abs/1607.05952) 

Citation
---------
If you use this code, please cite the following scientific article:
- L. Pappalardo and F. Simini, Data-driven generation of spatio-temporal routines in human mobility, Data Mining and Knowledge Discovery,
Volume 32, Issue 3, pp 787–829, 2018 (https://link.springer.com/article/10.1007/s10618-017-0548-4)

Required Python packages
------------------------
- random
- cPickle
- math
- scipy
- collections
- numpy
- sys
- os
- time
- csv

How to run DITRAS as standalone
-----------------
To run the code of DITRAS, download all the folder in this repository and write on the terminal a command in the following format:   
`python DITRAS.py n_agents time_slots spatial_tessellation od_matrix diary_generator_1hour.pkl filename`, where:
- **n_agents** is an integer indicating the number of agents to simulate (e.g., 10000)
- **time_slots** is the length of the time period in hours (e.g., 720, that is 1 month)
- **spatial_tessellation** is the name of a file where the weighted spatial tessellation is stored. The format of the `spatial_tessellation` file must be the following: `latitude,longitude,relevance`. Latitude and longitude are float numbers indicating the geographic position of a location and relevance is an integer indicating the importance of the location (e.g., the number of residents in the locations or the number of calls made in that location during a given period). We provide you an example of spatial tessellation in the Italian regione of Trentino, stored in the file `location2info_trentino`. This spatial tessellation is released under a Open Data Commons Open Database License (ODbL) and it is obtained from the publicly available dataset described in this paper [http://www.nature.com/articles/sdata201555#data-records](http://www.nature.com/articles/sdata201555#data-records).  
- **od_matrix** is the name of a file where the origin-destination matrix based on the spatial_tessellation is stored. The origin-destination matrix is used by DITRAS to decide the movements of the synthetic agents. If the file specified does not exist in the current folder, DITRAS will automatically compute the origin-destination matrix and store it in a file named `od_matrix`.
- **diary_generator_1hour.pkl** is the diary generator we computed for you, stored as a pickle file. If you have real trajectory data, you can compute your own diary generator using the Python script `diary_generator_builder.py`.


For example type the following command on the terminal to start your DITRAS simulation:
~~~
python DITRAS.py 10000 720 location2info_trentino od_matrix.pkl diary_generator_1hour.pkl trajs_10000_720.csv 
~~~
DITRAS will simulate the mobility of 10,000 agents for 720 hours (1 months), using the `location2info_trentino` weighted spatial tessellation and the diary generator `diary_generator_1hour.pkl`. The produced synthetic trajectories will be stored in a file named `trajs_10000_720.csv`. DITRAS will also compute the origin destination matrix `od_matrix.pkl` the first time you run the command. 

How to run DITRAS as library
-----------------------------
First, read the spatial tessellation from the corresponding file and load it in a dictionary, using the function `load_spatial_tessellation`. In this repository we provide an example of spatial tessellation regarding the Italian region of Trentino, it is stored in the file `location2info_trentino`. Second, initialize a DITRAS object with the parameters you prefer. Finally, start the execution of DITRAS by using the method `start_simulation`.
~~~
spatial_tessellation = load_spatial_tessellation('location2info_trentino')

ditras = DITRAS(n_agents=10000, length=720, diary_generator=MD(filename='diary_generator_1hour.pkl'),
                 trajectory_generator=dEPR(rho=0.6, gamma=0.21), filename='trajs_10000_720.csv')

ditras.start_simulation(spatial_tessellation, od_matrix_file='od_matrix.pkl')
~~~
Run the `ditras_example.ipynb` notebook in this repository to try different executions of DITRAS.


Output of DITRAS
----------------
DITRAS produces a file with the following format:
~~~
agent_id,location_id,time_slot
~~~
where:
- **agent_id** is an identifier of the agent
- **location_id** is the identifier of the location visited by the agent, as specified in the spatial tessellation file
- **time_slot** is the time slot of the visit, i.e., the hour when the agent visited the location






