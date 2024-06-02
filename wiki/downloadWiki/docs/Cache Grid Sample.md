# Cache Sample

This sample displays movies from Netflix and allows filtering on the average rating and year released.

Some techniques demonstrated include:

- How to configure the cache to store and prefetch items.
- How to use filterForward and filterBack to populate pages.

The sample file is [NetflixCache.htm](./Cache%20Grid%20Sample_NetflixCache.htm), which can be added to the [Local Service Sample](./Local%20Service%20Sample.md) to run.

**Note:** The sample does not include an implemenation for "Last Page" because the Netflix service does not implement $count for JSONP. $count is supported in [DataServicesJSONP v0.3](http://archive.msdn.microsoft.com/DataServicesJSONP/Release/ProjectReleases.aspx?ReleaseId=5660).
