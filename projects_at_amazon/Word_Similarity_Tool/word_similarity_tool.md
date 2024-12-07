## **Similar Product Recommendation Tool Project Background**

When packaging Amazon products, using simplified packaging methods (e.g., paper bags or flexible packages) instead of cardboard boxes can significantly reduce package volume and material costs, leading to lower transportation costs. Increasing the proportion of shipments using simplified packaging is a critical metric for achieving our team goals. However, ensuring the protection of shipped packages by applying appropriate packaging and reducing DPMO (Defects per Million Opportunities) is also a key initiative that must be prioritized.  

Our quality team assigns annotation tags to ASINs (Amazon Standard Identification Number, i.e. product ID) that are considered liquid or fragile through internal registering system, ensuing those ASINs to be shipped in corrugated boxes instead of simplified package to avoid item damage or liquid leakage. The traditional process involves our quality team using a query to extract liquid/fragile ASIN candidates from product attribute database, then apply the annotation tag in the system. The query limits the candidates based on product category, brand name, and specific keywords representing liquid/fragile properties, such as "gallon", "ml", and "dish".  

Unfortunately, the query-based approach has limitations in precisely extracting only liquid/fragile eligible ASINs. The query often captures ASINs that contain the specific keywords but do not actually belong to liquid or fragile. For instance, the abbreviation "ml" can match words like "Ha<u>ml</u>et" that are unrelated to liquid items. As a result, our quality team has to manually inspect these "wrongly flagged ASINs" later and remove the annotation tag to assign the package type back to flexible packaging.

## **Similar Product Recommendation Tool as a Solution**

To reduce the effort required to find wrongly-flagged ASINs from a vast candidates, I have developed a Similar Product Recommendation Tool. This tool enables users to search for ASINs associated with a given keyword input (ASIN title or description), allowing them to identify the similar ASINs. This could lead to de-annotate non-liquid/non-fragile ASINs (wrongly flagged ones) more efficiently than the manual, one-by-one inspection process.  
  
The tool utilizes LLM (large language model) to calculate the cosine similarity of the TF-IDF (Term Frequency-Inverse Document Frequency) vector between the input keywords and the specific ASIN title, and then extracts the top 10 most similar ASINs from the ASIN pool. The searching algorithm behind the tool identifies important words in the text based on frequency and uniqueness. Then, compares the input keywords with each ASIN title/description to estimate the closeness between the two texts in the word vector space. This approach can identify similar ASINs more accurately than the traditional word-matching method as query does.  
  
When the user wants to find a set of ASINs with a specific feature, they can extract a bulk of related ASINs using the search word, which helps de-annotate the ASINs all at once. For example, if hair wax items are defined as non-liquid but registered as liquid, the user can search for ASIN title like "styling wax" or "hair oil" to extract the top 10 hair wax-related ASINs to remove those annotation tag efficiently.

**Similar Product Recommendation Tool image**
(Noting that this is not a real image of an actual system, but is resemble to the original user interface)

<img src="/assets/img/Word_Similarity_Tool/UI_image.png" width="800"/>

## **How It Functions**

Users can easily access the tool by clicking the HTML file available in the shared internal folder. The keyword entered in the search box is sent via AWS API Gateway to a Lambda function, which forwards it to an AWS EC2 server. The EC2 server performs similarity calculations for the keyword, returning the top 10 ASINs with the highest similarity. These results are then displayed on the user’s screen, showing related products.

A key improvement in the system is that the ASIN pool on the EC2 server is pre-processed and stored as TF-IDF vectors, enabling instant similarity calculations within a Python-based Flask server. Additionally, hosting both Lambda and EC2 within the same VPC (Virtual Private Cloud) allows the Lambda function to directly execute commands on the EC2 server, streamlining the process.

**System Architecture**
<img src="/assets/img/Word_Similarity_Tool/System_architecture.png" width="800"/>

## **Potential Other Usage**

The Similar Product Recommendation Tool offers the potential for broader applications in the future.

One key area is identifying SIOC (Ship In Own Container) packaging types, which do not require additional Amazon packaging materials as they are already boxed by vendors upon delivery. This eliminates the need for extra packaging, improving truck loading efficiency and reducing material costs. The Similar Product Recommendation Tool can efficiently identify SIOC-eligible products from Amazon’s vast inventory of 50 million items by searching the items having similar features to SIOC. For example, beverage products like water bottles are often delivered pre-packaged in corrugated box, making them strong candidates for SIOC certification. By targeting beverage-related keywords, the tool can help identify water bottles that are not yet certified, enabling proactive cost reduction in transportation.

Another potential use case involves mitigating future risks for damaged ASINs. When a specific ASIN is damaged or marked as compromised, similar ASINs sharing common characteristics may also be susceptible to damage. Damaged products are typically discarded or replaced within the warehouse before being shipped to customers. By utilizing the tool to identify ASINs with similar features, packaging assignments can be proactively adjusted to prevent damage before items reach customers. This approach not only avoids the delivery of damaged products but also contributes to improved long-term customer satisfaction.

## **Future Development**

For making the tool more useful and convenient to expand the tool usage, the tool can be developed further.

- Build dynamic system for accessing up-to-date ASIN data

  - Current system searches the similar ASIN from static ASIN pool which was extracted beforehand. Therefore, recommended ASIN suggested by the tool might be obsolete or limited. Also, as the current TF-IDF model is fitted with those static ASIN prepared beforehand, model update can only be done manually. We can build extensive system architecture for maintenancability and accessibility, and enable it to search newly registered ASIN recently.

- Bulk ASIN Search

  - Currently, the tool can only search and process one ASIN at a time. We need to enable the ability to search and process multiple ASINs together in a bulk manner.

- Downloadable Search Results as Excel File

  - The current tool only displays similar ASINs, but we need to add the functionality to download the search results locally to the user's PC as an Excel file.

- Add more columns for recommended ASINs by the tool

  - There is only information about ASIN and its title appeared in the prediction output, we can add other useful columns such as annotation status, SIOC flag, or product category information.

## **The Adopted Algorithm**

Adopted method to calculate the similarity is fundamental in general search engines and text analysis, helping find similarities between two different texts. The logic consists of two different logic: TF-IDF and Cosine similarity.

- TF-IDF (Term Frequency - Inverse Document Frequency)

Term Frequency (TF) measures how often a word appears in a document. If a word appears frequently, it’s important for that specific document. Inverse Document Frequency (IDF) measures how unique a word is across a collection of documents. If a word is rare across many documents, it has a higher IDF value. TF-IDF Score can be calculated by multiplying TF by IDF. A high TF-IDF score means the word is important in a specific document but not common in all documents. For instance, in a book about "cats," the word "cat" appears often (high TF) but isn't common across all books (high IDF), giving it a high TF-IDF score.

- Cosine Similarity

Cosine Similarity measures how similar two documents are by representing each document as a vector based on their word content. It calculates the cosine of the angle between two vectors. If the angle is small (vectors point in the same direction), the documents are similar. If the angle is large (vectors point in different directions), the documents are different. For instance, two articles about "electric cars" will have similar vectors, resulting in high cosine similarity, meaning they cover similar topics.
