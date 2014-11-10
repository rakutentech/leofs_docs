.. =========================================================
.. LeoFS documentation
.. Copyright (c) 2012-2014 Rakuten, Inc.
.. http://leo-project.net/
.. =========================================================

.. _aws-sdk-java-label:

Getting Started with Java
-------------------------

Getting AWS SDK for Java
^^^^^^^^^^^^^^^^^^^^^^^^

URL: `SDK for Java <http://aws.amazon.com/sdkforjava/>`_

.. note:: LeoFS domains are ruled by :ref:`this rule <s3-path-label>`. You need to set 'Proxy Host' and 'Proxy Port' with ClientConfiguration class.


Example usage
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: java

  import java.io.BufferedReader;
  import java.io.File;
  import java.io.FileOutputStream;
  import java.io.IOException;
  import java.io.InputStream;
  import java.io.InputStreamReader;
  import java.io.OutputStreamWriter;
  import java.io.Writer;
  import java.util.UUID;

  import com.amazonaws.AmazonClientException;
  import com.amazonaws.AmazonServiceException;
  import com.amazonaws.auth.AWSCredentials;
  import com.amazonaws.auth.BasicAWSCredentials;
  import com.amazonaws.services.s3.AmazonS3;
  import com.amazonaws.services.s3.AmazonS3Client;
  import com.amazonaws.services.s3.model.GetObjectRequest;
  import com.amazonaws.services.s3.model.ListObjectsRequest;
  import com.amazonaws.services.s3.model.PutObjectRequest;
  import com.amazonaws.services.s3.model.Bucket;
  import com.amazonaws.services.s3.model.S3Object;
  import com.amazonaws.services.s3.model.ObjectListing;
  import com.amazonaws.services.s3.model.S3ObjectSummary;
  import com.amazonaws.ClientConfiguration;
  import com.amazonaws.Protocol;

  public class LeoFSSample {
      public static void main(String[] args) throws IOException {
          /* ---------------------------------------------------------
           * You need to set 'Proxy host', 'Proxy port' and 'Protocol'
           * --------------------------------------------------------- */
          ClientConfiguration config = new ClientConfiguration();
          config.setProxyHost("localhost"); // LeoFS Gateway's Host
          config.setProxyPort(8080);        // LeoFS Gateway's Port
          config.withProtocol(Protocol.HTTP);

          final String accessKeyId = "YOUR_ACCESS_KEY_ID";
          final String secretAccessKey = "YOUR_SECRET_ACCESS_KEY";

          AWSCredentials credentials = new BasicAWSCredentials(accessKeyId, secretAccessKey);
          AmazonS3 s3 = new AmazonS3Client(credentials, config);

          final String bucketName = "test-bucket-" + UUID.randomUUID();
          final String key = "test-key";

          try {
              // Create a bucket
              s3.createBucket(bucketName);

              // Retrieve list of buckets
              for (Bucket bucket : s3.listBuckets()) {
                  System.out.println("Bucket:" + bucket.getName());
              }

              // PUT an object into the LeoFS
              s3.putObject(new PutObjectRequest(bucketName, key, createFile()));

              // GET an object from the LeoFS
              S3Object object = s3.getObject(new GetObjectRequest(bucketName, key));
              dumpInputStream(object.getObjectContent());

              // Retrieve list of objects from the LeoFS
              ObjectListing objectListing =
                  s3.listObjects(new ListObjectsRequest().withBucketName(bucketName));

              for (S3ObjectSummary objectSummary : objectListing.getObjectSummaries()) {
                  System.out.println(objectSummary.getKey() +
                                     "Size:" + objectSummary.getSize());
              }

              // DELETE an object from the LeoFS
              s3.deleteObject(bucketName, key);

              // DELETE a bucket from the LeoFS
              s3.deleteBucket(bucketName);

          } catch (AmazonServiceException ase) {
              System.out.println(ase.getMessage());
              System.out.println(ase.getStatusCode());
          } catch (AmazonClientException ace) {
              System.out.println(ace.getMessage());
          }
      }

      private static File createFile() throws IOException {
          File file = File.createTempFile("leofs_test", ".txt");
          file.deleteOnExit();

          Writer writer = new OutputStreamWriter(new FileOutputStream(file));
          writer.write("Hello, world!\n");
          writer.close();

          return file;
      }

      private static void dumpInputStream(InputStream input) throws IOException {
          BufferedReader reader = new BufferedReader(new InputStreamReader(input));
          while (true) {
              String line = reader.readLine();
              if (line == null) break;
              System.out.println(line);
          }
      }
  }

