#include "include/hdf5.h"
#include <stdio.h>
#include <stdlib.h>
#include <iostream>
#include "time.h"
#include <sstream>

std::string INFILE;
std::string OUTFILE;
std::string DATASET;
std::string DOY; //Day Of Year

void hdf2csv( )
{
	std::string strHeader = "x,y,date,chlor_a\n";
	std::string temp = "";
	
	float ** 	data; /* HDF value*/
	FILE *		ofile; /* outfile(csv) */
	hid_t 		file_id; /* HDF file */
	hid_t		dataset_id; /* HDF dataset id*/
	hid_t		space; /* HDF space */
	herr_t 		status;
	hsize_t 	dim[2]; /* HDF dimension (y,x) */
	char		dname[256]; /* dataset name. ex) /chlor_a */

	/********** Read hdf5 Start **********/
	sprintf(dname, "/%s", DATASET.c_str());
	
	file_id = H5Fopen(INFILE.c_str(), H5F_ACC_RDONLY, H5P_DEFAULT);
	dataset_id = H5Dopen(file_id, dname, H5P_DEFAULT);
	space = H5Dget_space(dataset_id);
	H5Sget_simple_extent_dims(space, dim, NULL);

	printf("dim[0]: %d, dim[1]: %d\n", dim[0], dim[1]);
	data = (float **) malloc (sizeof(float *) * dim[0]);
	data[0] = (float *) malloc( sizeof(float) * dim[0] * dim[1]);

	for (int i=0; i<dim[0]; i++)
	{
		data[i] = data[0] + i*dim[1];
	}

	status = H5Dread(dataset_id, H5T_NATIVE_FLOAT, H5S_ALL, space, H5P_DEFAULT, &data[0][0]);
	/********* Read hdf5 End *********/

	/********* Write csv start *********/
	ofile = fopen(OUTFILE.c_str(), "w");
	fwrite( strHeader.c_str(), sizeof(char), strHeader.length(), ofile);

	for (int y=0; y < dim[0]; y++)
	{
		for (int x=0; x < dim[1] ; x++)
		{
			if (data[y][x] != -32767 && data[y][x] != 65535)
			{
				char buff[50];
				sprintf( buff, "%d, %d, %s, %0.3f\n", x,y,DOY.c_str(),data[y][x]);
				temp = buff;
				fwrite( temp.c_str(), sizeof(char), temp.length(), ofile);
			}
		}
	}
	/********* Write csv end ********/

	/********** close File start **********/
	status = H5Dclose(dataset_id);
	status = H5Sclose(space);
	status = H5Fclose(file_id);

	free(data[0]);
	free(data);
	
	fclose(ofile);
	/********** close File end **********/
}

int main(int argc, char **argv)
{
	clock_t 	before;
	double 		result;
	
	before = clock();

	if (argc < 9) 
	{
		printf("[ERROR] An insufficient number of arguments\n");
		return 0;
	}
	else if (argc > 9)
	{
		printf("[ERROR] Too many arguments \n");
		return 0;
	}

	std::ostringstream os;
	for (int i=0; i<argc; i++)
	{
		os << argv[i] << " ";
	}

	std::istringstream is(os.str());

	while( !is.eof() )
	{
		std::string tag;
		is >> tag;

		if (tag == "-o")
			is >> OUTFILE;
		if (tag == "-i")
			is >> INFILE;
		if (tag == "-d")
			is >> DATASET;
		if (tag == "-t")
			is >> DOY;
		if (is.fail())
			break;
	}

	hdf2csv();

	result = (double)(clock() - before) / CLOCKS_PER_SEC;
	printf("time: %5.2f sec \n", result); 

	return 0;
}
