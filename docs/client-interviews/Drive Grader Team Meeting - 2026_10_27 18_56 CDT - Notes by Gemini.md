Oct 27, 2026
### **Summary**

Cursor verification and mobile app targets and database designs discussed via meetings.

**Cursor Account And Application Requirements**  
Account verification confirmed active usage. Texas state requirements detailed 44 hours for teen drivers.

**Road Test And Database Design**  
Grade sheet handled DL40 maneuvers. Database design supported driving schools as organizations.

**Deployment Stack And Remote Development**  
Staging environment updated automatically via branch pushes. Tailscale VPN utilized for remote debugging.

### **Decisions**

## Aligned

* **Adoption of GitHub Project for management** The team agreed to utilize GitHub Project within the client's repository for project management.

* **Migration of documentation to repository** The participants agreed to move all project documentation into the client's repository.

* **Mandatory code review policy for pull requests** The team established a standard requiring at least one code review before merging any pull request.

### **Next steps**

- [ ] Develop Progress Tracker: Develop a tool to track user progress through instructional requirements.

- [ ] Add PDF Export: Implement functionality to generate and export student progress reports as PDF files.

- [ ] Migrate Project Documentation: Move all relevant project documentation files into the central repository.

- [ ] Enable Manual Entry: Implement a feature allowing users to manually log observation driving hours within the system.

- [ ] Require Code Reviews: Request at least 1 person to review every pull request before merging. This prevents git conflicts and accidental code breakage.

- [ ] Migrate Documentation: Transfer all documentation to the Eric repository. Verify the accuracy of AI generated documentation after the move.

- [ ] Configure GitHub Project: Initialize a new project board to track and visualize progress on tasks. Ensure all work is visible to team members.

- [ ] Draft Use Case: Write the project use case for the documentation. Complete this task by tonight.

- [ ] Review Business Rules: Examine and update the project business rules within the documentation. Verify all updated content.

- [ ] Upload Meeting Notes: Save the meeting transcript to the Google Drive folder. Enable team access to the file.

### **Details**

**Cursor Account Verification**: Kanta Endo reported an issue with their Cursor account setup after switching laptops. Eric Brown checked the user activity dashboard and verified active accounts, confirming Kanta Endo's account usage under the email \`kalin2023@gmail.com\`. Eric Brown noted that they use a maximum plan on a pod to burn credits.

---

**Mobile Application Target Users and State Requirements**: Kanta Endo asked about the target user base for the mobile application, and Eric Brown explained that while initially deployed for users in Texas, it could theoretically expand to other providers or schools. Eric Brown detailed the Texas state requirements for teen drivers under 18, known as "seven and seven" (7 hours of instruction and 7 hours of observation) plus 30 hours of logged driving (capped at 1 hour per day over at least 6 months), totaling 44 hours. Eric Brown discussed potential pricing models, suggesting the app could be packaged as a value-added service with parent-taught driver education programs.

---

**Road Test Grade Sheet and Custom Routes**: Eric Brown explained that Texas teens require a completion certificate, impact video certificate, license, and a 30-hour driving log for road tests, though the log is rarely reviewed. Eric Brown noted that the application was designed to handle the official Texas Department of Public Safety road test grade sheet, known as the DL40, which tracks maneuvers like approach to corner, backup 60 feet, and parallel parking. Because test routes vary by location, Eric Brown stated that the app allows users to create custom routes and reorder events to make grading easier for instructors and parents, who often lack formal training. Eric Brown also mentioned future enhancements like integrating automatic metrics for hard braking, acceleration, live speedometers, and GPS tracking.

---

**Organization Tabs and Database Design**: Kanta Endo inquired about the user and organization tabs visible in the UI. Eric Brown confirmed that organizations represent driving schools in the database design, supporting a software-as-a-service component where administrators can create custom routes while end users have restricted access.

---

**Project Management and Documentation Repository**: Kanta Endo proposed using GitHub Projects for project management and requested to consolidate all documentation into Eric Brown's repository. Eric Brown agreed, telling Kanta Endo they could use whatever project management tools and repository structure they preferred.

---

**Deployment, Staging Environment, and Technology Stack**: Eric Brown explained the deployment process, noting that pushing changes to the staging branch automatically updates the live staging environment containing both the application and the Node API. Regarding the technology stack, Eric Brown and Kanta Endo discussed using Quasar, Capacitor, or Flutter, with Eric Brown suggesting Flutter as an option for native mobile app store deployment, while noting the app currently lacks deep code complexity.

---

**Parent Portal and Real-Time Tracking**: Eric Brown brought up the possibility of a web portal allowing parents to track their student's driving in real time, comparing the feature to ride-sharing applications like Uber. Eric Brown clarified that this feature is not a strict requirement but a potential value-add for monitoring drivers.

---

**Tracking Observation Hours and Manual Logging**: Kanta Endo asked how to handle the 7 hours of observed driving where students ride as passengers while another student drives, noting that tracking differs from active driving. Eric Brown described how driving hours are typically stacked in pairs during school programs and acknowledged that adding a manual time-entry feature for observation hours would be beneficial. Eric Brown and Kanta Endo agreed that incorporating a manual entry method and PDF progress reporting would support state tracking needs.

---

**Progress Reporting and Dark Mode Feature**: Kanta Endo reported adding a dark mode feature accessible via a moon icon in the top right of the UI, which had already been pushed to the staging environment. Eric Brown acknowledged the update and mentioned progressive web application installation for mobile devices.

---

**Tailscale VPN and Remote Development Testing**: Eric Brown explained the utility of Tailscale, a VPN tool used for remote development and testing across multiple devices without needing to push code to GitHub. Kanta Endo questioned the necessity of setting up Tailscale given that the project is already deployed via GitHub. Eric Brown clarified that while Tailscale is a handy tool for remote debugging and interacting with local AI coding sessions, its use is optional, leading Kanta Endo to conclude they would skip setting it up.

---

**Staging Banner and Environment Indicators**: Syrah Omar asked for guidance regarding the staging indicator banner in the code and whether it should be maintained for the TCU setup. Eric Brown explained that staging banners are useful to prevent accidental production changes during training or testing, but granted Syrah Omar full freedom to remove, modify, or keep the banner as needed.

---

**Employee Tracking, Facial Recognition, and Location Services**: Eric Brown noted that tracking app usage allows the office to monitor driver locations and discussed broader technology trends like facial recognition and home security systems. Regarding technical requirements, Eric Brown emphasized that mobile application users must enable location services for the app to function properly.

---

**Post-Meeting Team Coordination and Deliverables**: After the meeting concluded, Kanta Endo and Syrah Omar discussed team checkpoints, upcoming demos for their teaching assistant, and recent code pull requests. Kanta Endo requested that team members request code reviews on pull requests to avoid conflicts, and agreed to handle documentation updates and use-case generation while coordinating file transfers to Eric Brown's repository.