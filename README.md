# Country-Data-Project
This project analyzes the given country data and converts it into into a readable, organized format:
AE for 2015-2019
2015	2016	2017	2018	2019	
100.0	100.0	100.0	100.0	100.0	
This is the "Access to electricity (% of population)" for Australia
Minimum: 100.0
Maximum: 100.0
Trending: no trend

Here are some key code samples: 

# public static void main(String[] args) throws FileNotFoundException{
		File inputFile = new File("CountryDataSet.csv");
		Scanner input = new Scanner(inputFile);   
		String line = input.nextLine();
		System.out.print(line); 
		String[] splitLine = line.split(",");
		String series = splitLine[0];
		line = input.nextLine();
		splitLine = line.split(",");
		int count=splitLine.length-1;
		//int[] years = new int[count];  //replaced with arrayList
		ArrayList<Integer> years = new ArrayList<Integer>();
		for (int i=0;i<count;i++) {
			years.add(Integer.parseInt(splitLine[i+1]));
		}
    ...
  The main method first reads a file. A scanner is then created to "read" the file and store the years of data within an ArrayList field called "years" which exists in a separate, "Country" class .  
  
 # main method (continued)
      while (input.hasNextLine()) {
			line = input.nextLine();
			splitLine = line.split(",");
			String countryName = splitLine[0];
			ArrayList<Double> values = new ArrayList<Double>();
			for (int i=0;i<count;i++) {
				values.add(Double.parseDouble(splitLine[i+1]));
			}
			Country myCountry = new Country(countryName, series, years, values);
			String acronym = myCountry.getAcronym();
			System.out.println(acronym + " for " + years.get(0) + "-" + years.get(years.size()-1));
			System.out.println(myCountry +"\n");
		} //end of while
		input.close();//last line in the main method 
	}
once the while loop is entered, it reads the values for the respective years and stores them in an ArrayList field called "values". These values, along with the country name, years, and series are then passed as parameters into the Country constructor. The title info is then printed for each country object created. 

# public static ArrayList<String> getListBasedOnTrend(ArrayList<Country> countries, String trendType){
		if(! ((trendType.equals("down"))|(trendType.equals("up"))|(trendType.equals("no trend")))){
			throw new IllegalArgumentException(trendType + " is not a valid argument");
		}
		ArrayList<String> identikit = new ArrayList<String>();
		for(int i=0;i<countries.size();i++) {
			if(countries.get(i).getTrend().equals(trendType)) {
				identikit.add(countries.get(i).getCountry());
			}
		}return identikit;
	}
This method within the CountryClient class identifies whether or not there is a trend of decreasing values or increasing values and stores those countries with identical trends in an ArrayList.
                                        
# public String toString() {
		String output="";
		for (int i=0; i < years.size();i++) {
			output +=years.get(i)+"\t";
		}
		output +="\n";
		for (int i=0; i < values.size();i++) {
			double value =Math.round(values.get(i)*100.0)/100.0;
			output +=value+"\t";
		}
		
		if(series.indexOf("(")<0) {
			String test = series.substring(series.indexOf("(")+1);
			if(test.indexOf("(")<0) {
				series = series.substring(0,series.indexOf("("));
			}else {
				String[] placeholder = series.split("(");
				series = "";
				for(int i = 0;i<placeholder.length;i++) {
					if(placeholder[i].indexOf(")")<0) {
						series+=placeholder[i] + " ";
					}
				}
				
			}
		}
		output +="\nThis is the \""+series+"\" for "+name+"\n";
		output +="Minimum: "+min()+"\n"+"Maximum: "+max();
		output+="\n"+"Trending: "+getTrend();
		return output;
	}
This method converts the values, years, and series of the country object into the correct format. It utilizes for loops to print the years, then the appropraite values for each year. It also calls other methods to print out info such as the min and max values and the trend. 
                                           
