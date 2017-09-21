# Build Docker Image
'''git clone https://github.com/hmesha/docker-tutorial.git'''
'''cd docker-tutorial'''
'''docker build -t hmesha/java8 java8/'''
'''docker build -t hmesha/activemq5 activemq5/'''

# Run Docker Image Container

docker run -d --name activemq5 -p 8161:8161 -p 61616:61616 -p 61613:61613 -p 61617:61617 hmesha/activemq5