# Content Delivery Network (CDN) 🌐

## 1. **What is a CDN?**  
   - Globally distributed network of proxy servers.  
   - **Purpose**: Serve content closer to users.  
   - **Benefits**:  
     - Faster delivery by reducing latency.  
     - Offloads requests from the origin server.  
   - **Examples**: CloudFront, Akamai.

## 2. **Types of CDNs**  
   ### A. Push CDN 📤  
   - **Mechanism**: Content manually uploaded to CDN servers.  
   - **Advantages**:  
     - Efficient for infrequently updated content.  
     - Low traffic results in minimized redundant traffic.  
   - **Disadvantages**:  
     - Requires manual updates.  
     - Higher storage costs.  
   - **Best For**: Small-scale or low-traffic sites.  

   ### B. Pull CDN 📥  
   - **Mechanism**: Fetches content from the origin on demand.  
   - **Advantages**:  
     - Automatic caching based on user requests.  
     - Lower storage costs.  
   - **Disadvantages**:  
     - Slower for first-time requests.  
     - Cache expiration may cause redundant fetches.  
   - **Best For**: High-traffic or frequently updated sites.

## 3. **How CDNs Work**  
   - Integrates with DNS resolution for server selection.  
   - **Caching**:  
     - Static content (HTML, CSS, JS).  
     - Dynamic support (CloudFront, etc.).  
   - **TTL (Time to Live)**: Governs cache refresh frequency.

## 4. **Applications of CDNs**  
   - **Static Content Delivery**: Images, scripts, videos.  
   - **Dynamic Content Optimization**: Some services support dynamic requests.  
   - **Load Balancing**: Geolocation and latency-based traffic routing.  
   - **Security Enhancements**: Protect against DDoS and provide redundancy.

## 5. **Advantages of CDNs**  
   - Improves site performance by reducing latency.  
   - Handles high traffic without overloading origin servers.  
   - Reduces bandwidth costs.  

## 6. **Disadvantages and Challenges**  
   - **Costs**: Can be significant for high-traffic sites.  
   - **Content Staleness**: Updates may not propagate immediately.  
   - **Setup Complexity**: Requires configuration of URLs and cache rules.

## 7. **Further Reading**  
   - [Globally Distributed Content Delivery](https://figshare.com/articles/Globally_distributed_content_delivery/6605972)  
   - [Push vs. Pull CDNs](https://www.travelblogadvice.com/technical/the-differences-between-push-and-pull-cdns)  