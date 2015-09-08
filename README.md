# elasticsearch very high CPU usage

if you are using marvel plugin, mavel is creating a one big index everyday, so fix to release memory and cpu usage is delete all marvel indexes.



public class IndicesDelete
	{
		public static void DeleteMarvelIndices()
		{
			ElasticClient client = ElasticSearch.Elasticsearch.Client;
			ICatResponse<CatIndicesRecord> indexses = client.CatIndices();

			foreach (CatIndicesRecord item in indexses.Records)
			{
				string indexName = item.Index;

				try
				{
				if (Regex.IsMatch(indexName, "marvel"))
					{
						if (client.IndexExists(x => x.Index(indexName)).Exists)
						{
							IIndicesResponse deleteInex = client.DeleteIndex(x => x.Index(indexName));
							if (deleteInex.Acknowledged)
							{
								Console.WriteLine("Deleted marvel index {0}", indexName);
							}
							else
							{
								Console.WriteLine("Delete index error {0} {1} {2}", indexName, deleteInex.ServerError.Status,
									deleteInex.ServerError.Error);
							}
						}
					}
				}
				catch (Exception ex)
				{
					Console.WriteLine("Delete index error {0}", indexName);
				}
			}
		}
	}
