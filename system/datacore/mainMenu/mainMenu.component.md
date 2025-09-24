# Main Menu Component

```jsx
function MainMenuComponent() {
    const [activeTab, setActiveTab] = dv.React.useState('tab1');
    
    const tabs = [
        { id: 'tab1', label: 'Dashboard', component: 'tab1.viewer.md' },
        { id: 'tab2', label: 'Notes & Queries', component: 'tab2.viewer.md' },
        { id: 'tab3', label: 'Analytics', component: 'tab3.viewer.md' }
    ];
    
    const TabContent = ({ tabId }) => {
        switch(tabId) {
            case 'tab1':
                return dv.React.createElement('div', { 
                    dangerouslySetInnerHTML: { 
                        __html: '<div class="tab-content"><h3>Dashboard Overview</h3><div class="content-section"><p>Welcome to your personal dashboard! This is the main overview tab.</p><div class="placeholder-widget"><h4>Recent Activity</h4><p>Your latest notes and modifications will appear here.</p></div></div></div>' 
                    } 
                });
            case 'tab2':
                return dv.React.createElement('div', { 
                    dangerouslySetInnerHTML: { 
                        __html: '<div class="tab-content"><h3>Notes & Queries</h3><div class="content-section"><p>This tab contains your note management and query tools.</p><div class="placeholder-widget"><h4>Quick Search</h4><p>Advanced search functionality will be implemented here.</p></div></div></div>' 
                    } 
                });
            case 'tab3':
                return dv.React.createElement('div', { 
                    dangerouslySetInnerHTML: { 
                        __html: '<div class="tab-content"><h3>Analytics & Insights</h3><div class="content-section"><p>Analyze your knowledge base with powerful insights.</p><div class="placeholder-widget"><h4>Writing Stats</h4><p>Track your daily writing progress and habits.</p></div></div></div>' 
                    } 
                });
            default:
                return dv.React.createElement('div', {}, 'Tab not found');
        }
    };
    
    return dv.React.createElement('div', { className: 'main-menu-container' },
        dv.React.createElement('div', { className: 'tab-navigation' },
            tabs.map(tab => 
                dv.React.createElement('button', {
                    key: tab.id,
                    className: `tab-button ${activeTab === tab.id ? 'active' : ''}`,
                    onClick: () => setActiveTab(tab.id)
                }, tab.label)
            )
        ),
        dv.React.createElement('div', { className: 'tab-content-container' },
            dv.React.createElement(TabContent, { tabId: activeTab })
        ),
        dv.React.createElement('style', {}, `
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
        `)
    );
}

return { MainMenuComponent };
```