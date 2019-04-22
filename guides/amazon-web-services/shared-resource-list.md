---
description: Track how services use shared resources so that they don’t conflict.
---

# Shared resource list

Redis supports up to 16 separate databases. We combine Redis users into the same cluster because clusters are expensive / overkill on their own for _e.g._ just tracking Sidekiq jobs.

## AppsStagingRedis

### **Databases:**

0: _unused_  
1: ContactForm  
2: Access Boston

## AppsProdRedis

### **Databases:**

0: _unused_  
1: ContactForm  
2: Access Boston

