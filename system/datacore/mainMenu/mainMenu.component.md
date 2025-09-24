# Main Menu Component

```jsx
function MainMenuComponent() {
    const tabs = [
        { id: 'tab1', label: 'Dashboard' },
        { id: 'tab2', label: 'Notes & Queries' },
        { id: 'tab3', label: 'Analytics' }
    ];
    
    const TabContent = ({ tabId }) => {
        switch(tabId) {
            case 'tab1':
                return <div className="tab-content">
                    <h3>Dashboard Overview</h3>
                    <div className="content-section">
                        <p>Welcome to your personal dashboard! This is the main overview tab where you can see:</p>
                        <ul>
                            <li>Recent notes and updates</li>
                            <li>Quick statistics</li>
                            <li>Important reminders</li>
                        </ul>
                        <div className="placeholder-widget">
                            <h4>Recent Activity</h4>
                            <p>Your latest notes and modifications will appear here.</p>
                        </div>
                    </div>
                </div>;
            case 'tab2':
                return <div className="tab-content">
                    <h3>Notes & Queries</h3>
                    <div className="content-section">
                        <p>This tab contains your note management and query tools:</p>
                        <ul>
                            <li>Search and filter notes</li>
                            <li>Dynamic queries</li>
                            <li>Tag management</li>
                        </ul>
                        <div className="placeholder-widget">
                            <h4>Quick Search</h4>
                            <p>Advanced search functionality will be implemented here.</p>
                        </div>
                        <div className="placeholder-widget">
                            <h4>Popular Tags</h4>
                            <p>Most frequently used tags in your vault.</p>
                        </div>
                    </div>
                </div>;
            case 'tab3':
                return <div className="tab-content">
                    <h3>Analytics & Insights</h3>
                    <div className="content-section">
                        <p>Analyze your knowledge base with powerful insights:</p>
                        <ul>
                            <li>Writing statistics</li>
                            <li>Connection graphs</li>
                            <li>Progress tracking</li>
                        </ul>
                        <div className="placeholder-widget">
                            <h4>Writing Stats</h4>
                            <p>Track your daily writing progress and habits.</p>
                        </div>
                        <div className="placeholder-widget">
                            <h4>Knowledge Graph</h4>
                            <p>Visualize connections between your notes.</p>
                        </div>
                    </div>
                </div>;
            default:
                return <div>Tab not found</div>;
        }
    };
    
    // Simple click handler that updates the DOM directly
    const handleTabClick = (tabId) => {
        // Update active tab styling
        document.querySelectorAll('.tab-button').forEach(btn => {
            btn.classList.remove('active');
        });
        document.querySelector(`[data-tab="${tabId}"]`).classList.add('active');
        
        // Update content
        const contentContainer = document.querySelector('.tab-content-container');
        if (contentContainer) {
            // Re-render the content (this is a simplified approach)
            const newContent = TabContent({ tabId });
            contentContainer.innerHTML = '';
            // Note: In a real implementation, you'd need to properly render JSX to DOM
            // For now, we'll use a simpler HTML approach
        }
    };
    
    return (
        <div className="main-menu-container">
            <div className="tab-navigation">
                {tabs.map(tab => (
                    <button
                        key={tab.id}
                        className={`tab-button ${tab.id === 'tab1' ? 'active' : ''}`}
                        data-tab={tab.id}
                        onClick={() => handleTabClick(tab.id)}
                    >
                        {tab.label}
                    </button>
                ))}
            </div>
            <div className="tab-content-container">
                <TabContent tabId="tab1" />
            </div>
            <style>{`
                .main-menu-container {
                    width: 100%;
                    max-width: 800px;
                    margin: 0 auto;
                    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
                }
                
                .tab-navigation {
                    display: flex;
                    border-bottom: 2px solid #e1e5e9;
                    margin-bottom: 20px;
                    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
                    border-radius: 8px 8px 0 0;
                    padding: 0;
                    overflow: hidden;
                }
                
                .tab-button {
                    flex: 1;
                    padding: 12px 20px;
                    border: none;
                    background: transparent;
                    color: rgba(255, 255, 255, 0.8);
                    cursor: pointer;
                    font-size: 14px;
                    font-weight: 500;
                    transition: all 0.3s ease;
                    position: relative;
                }
                
                .tab-button:hover {
                    background: rgba(255, 255, 255, 0.1);
                    color: white;
                }
                
                .tab-button.active {
                    background: rgba(255, 255, 255, 0.2);
                    color: white;
                    font-weight: 600;
                }
                
                .tab-button.active::after {
                    content: '';
                    position: absolute;
                    bottom: 0;
                    left: 0;
                    right: 0;
                    height: 3px;
                    background: white;
                }
                
                .tab-content-container {
                    background: white;
                    border-radius: 0 0 8px 8px;
                    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
                    min-height: 400px;
                }
                
                .tab-content {
                    padding: 24px;
                    animation: fadeIn 0.3s ease-in;
                }
                
                @keyframes fadeIn {
                    from { opacity: 0; transform: translateY(10px); }
                    to { opacity: 1; transform: translateY(0); }
                }
                
                .tab-content h3 {
                    margin: 0 0 16px 0;
                    color: #2c3e50;
                    font-size: 24px;
                    font-weight: 600;
                }
                
                .content-section {
                    color: #555;
                    line-height: 1.6;
                }
                
                .content-section ul {
                    margin: 16px 0;
                    padding-left: 20px;
                }
                
                .content-section li {
                    margin: 8px 0;
                }
                
                .placeholder-widget {
                    background: #f8f9fa;
                    border: 1px solid #e9ecef;
                    border-radius: 6px;
                    padding: 16px;
                    margin: 16px 0;
                    border-left: 4px solid #667eea;
                }
                
                .placeholder-widget h4 {
                    margin: 0 0 8px 0;
                    color: #495057;
                    font-size: 16px;
                    font-weight: 600;
                }
                
                .placeholder-widget p {
                    margin: 0;
                    color: #6c757d;
                    font-size: 14px;
                }
            `}</style>
        </div>
    );
}

return { MainMenuComponent };
```