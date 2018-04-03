# hdf2csv
* OS: CentOS6.5
* Build: <code>g++ -o hdf2csv hdf2csv.cpp -L$HOME/hdf2csv/lib -I$HOME/hdf2csv/include -lhdf5</code>
* Usage: <code>./hdf2csv -o A2016215_1D_sst.csv -i A2016215_1D.L3m_DAY_sst -d //l3m_data -t 2016215</code>
  * -i: input file
  * -o: output file
  * -d: dataset name
  * -t: date
