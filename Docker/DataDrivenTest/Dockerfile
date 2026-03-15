# Use the official Node.js 22 image as the base image
FROM node:22

# Set the working directory in the container
WORKDIR /app


# Install Newman globally
RUN npm install -g newman \
    && npm install -g newman-reporter-html newman-reporter-htmlextra

#Install Newman html report
RUN npm install -g newman-reporter-html newman-reporter-htmlextra


# Copy your Postman collection and environment files to the working directory, "." in the end means current directory, so all files should present in one place
COPY DataDrivenAPIsCollection.json .
COPY GoRestEnv.json .
COPY myuser.csv .

# Make newman the entrypoint (so whatever args you pass get appended)
ENTRYPOINT ["newman"]

# Put CMD on a single line (JSON array form)
CMD ["run","DataDrivenAPIsCollection.json","-e","GoRestEnv.json","-d","myuser.csv","-r","cli,htmlextra","--reporter-htmlextra-export","/app/newman/report.html"]