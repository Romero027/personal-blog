# Short Summaries

### [Early Detection of Configuration Errors to Reduce Failure Damage](https://www.usenix.org/system/files/conference/osdi16/osdi16-xu.pdf) - Xu et al., OSDI' 16

This paper is about detecting **latent configuration\(LC\) errors**. Some configuration parameters are neither used nor checked during normal operations, errors in their settings go undetected until their late manifestation\(e.g., under circumstances like error handling and fail-over\). 

```text
// Example
// Error handling function
static void call_techsup(int sig) {
    if (fork() == 0) {
        char* args[] = {“0911”, “SOS”};
    // dial_prog_path may be invalid, and even if you check the file existence and types
    // maybe you don't have the right permission!  
    int rv = execvp(dial_prog_path, args); 
    if (rv != 0)
        fprintf(stderr, “I’m sorry (%d)!”, errno);
    }
}
```

The authors proposes **PCheck**, which is a tool for enabling early detection of configuration errors. It can automatically generate configuration checking code based on the original program\(i.e., the intermediate representation of the programs\), and invoke them at the system initialization phase. To prevent side effects, PCheck validates the arguments of the call, but does not actually execute the call. \(e.g., replace open with access/stat\).

